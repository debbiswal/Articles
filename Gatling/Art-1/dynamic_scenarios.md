[:house:Home](https://github.com/debbiswal/Articles)

# Executing different scenarios dynamically in Gatling  

**Prerequisite :**  
Copy the attached org.json.jar.zip file to lib folder of gatling application , and unzip it.  
OR  
You can download it from http://www.java2s.com/Code/Jar/o/Downloadorgjsonjar.htm  


**JSON**  
Below is the JSON which will be passed as command line parameter to gatling.  
```
[
  {
    "scenario": "WithLatency100_Steady20sec",
    "url": "/",
    "inject": [ {  "type": "rampUsers",  "args": [10,5]}, {  "type": "nothingFor",  "args": [20]}, {  "type": "rampUsers", "args": [10,5]}, {  "type": "nothingFor", "args": [10 ]} ]
  }
]
```  

In the above example :  
scenario : is the scenario name  
url : is the url part which will be used with baseurl  
type = Injection type  
args=Arguments to Injection type  

Like :  
nothingFor : is used to make steady state for 20 seconds  
rampUsers : is used for ramping up 10 users  in 5 seconds  

for more settings  , check getInjectionStep method in CustomSimulation.scala  

As the above JSON is an array , we can implement multiple scenarios with custom url and injection pattern  


**Command**  
The command line argument will be passed with JAVA_OPTS , like below (note that escape characters will be used to format the JSON) :  
```
JAVA_OPTS="-DtestCases=[{\"scenario\":\"WithLatency100_Steady20sec\",\"url\":\"/\",\"inject\":[{\"type\":\"rampUsers\",\"args\":[10,5]},{\"type\":\"nothingFor\",\"args\":[20]},{\"type\":\"rampUsers\",\"args\":[10,5]},{\"type\":\"nothingFor\",\"args\":[10]}]}]" ./gatling.sh -s CustomerOrderV2.CustomSimulation
```

**Gatling script :**  
CustomSimulation.scala  
```
package CustomerOrderV2

import io.gatling.core.Predef._
import io.gatling.core.structure.PopulationBuilder
import io.gatling.core.controller.inject.InjectionStep
import io.gatling.http.Predef._
import io.gatling.jdbc.Predef._
import scala.concurrent.duration._
import scala.collection.mutable.ArraySeq
import org.json.JSONArray;
import org.json.JSONObject;

class CustomSimulation extends Simulation {
    val baseURL = "http://127.0.0.1:10001"
    val httpConf = http.baseURL(baseURL)
    val rawTestList = System.getProperty("testCases")

    def getInjectionStep(stepType:String, stepArgs:JSONArray) : InjectionStep = {
        stepType match {
            case "rampUsers" => rampUsers(stepArgs.getInt(0)) over(stepArgs.getInt(1))
            case "heavisideUsers" => heavisideUsers(stepArgs.getInt(0)) over(stepArgs.getInt(1))
            case "atOnceUsers" => atOnceUsers(stepArgs.getInt(0))
            case "constantUsersPerSec" => constantUsersPerSec(stepArgs.getInt(0)) during(stepArgs.getInt(1))
            case "rampUsersPerSec" => rampUsersPerSec(stepArgs.getInt(0)) to(stepArgs.getInt(1)) during(stepArgs.getInt(2))
            case "nothingFor" => nothingFor(stepArgs.getInt(0))
        }
    }

    def scnList() : Seq[PopulationBuilder] = {
        var testList = new JSONArray(rawTestList)
        var scnList = new ArraySeq[PopulationBuilder](testList.length())
        for(i <- 0 until testList.length()) {
            //For each scenario
            var testCase = testList.getJSONObject(i)
            val injectionSteps = testCase.getJSONArray("inject")
            var injectStepList = new ArraySeq[InjectionStep](injectionSteps.length())
            for(j <- 0 until injectionSteps.length()) {
                //For each injection step
                var step = injectionSteps.getJSONObject(j)
                var stepType = step.getString("type")
                var stepArgs = step.getJSONArray("args")
                injectStepList(j) = getInjectionStep(stepType, stepArgs)
            }                        

            var methodname = testCase.getString("scenario")
            var url = testCase.getString("url")            

             var scn = scenario(methodname)
             .forever{
                        exec(http(methodname)
                            .get(session => url)
                        )            
             }
            .inject(injectStepList:_*)

            scnList(i) = scn

        }
        scnList
    }

    setUp(scnList:_*).protocols(httpConf).maxDuration(1 minutes)
}
```  

*Note : You need to change the yellow highlighted section(baseurl,maxDuration) as per your need
If you are changing the package name and class name in CustomSimulation.scala then , remember to use same in command line.*


Happy Learning :smiley:  

[:house:Home](https://github.com/debbiswal/Articles)