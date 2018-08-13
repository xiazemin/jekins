# Mac环境中Jenkins的停止和启动命令

启动

sudo launchctl load /Library/LaunchDaemons/org.jenkins-ci.plist

停止

sudo launchctl unload /Library/LaunchDaemons/org.jenkins-ci.plist

java -jar /Applications/Jenkins/jenkins.war --httpPort=8087

[http://localhost:8087/](http://localhost:8087/)

Jenkins initial setup is required. An admin user has been created and a password generated.

Please use the following password to proceed to installation:



8b1f80e61bc2407199cf0f14a2465b09



This may also be found at: /var/root/.jenkins/secrets/initialAdminPassword

