# Install Certificate

## Java

```bash
keytool -import -alias <alias-name> -keystore $JAVA_HOME/lib/security/cacerts -file <path-to-cert>.pem -storepass changeit
```
