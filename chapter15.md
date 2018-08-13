# Mac环境中Jenkins的停止和启动命令

启动

sudo launchctl load /Library/LaunchDaemons/org.jenkins-ci.plist

停止

sudo launchctl unload /Library/LaunchDaemons/org.jenkins-ci.plist



java -jar /Applications/Jenkins/jenkins.war --httpPort=8087



http://localhost:8087/

