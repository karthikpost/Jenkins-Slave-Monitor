import hudson.slaves.*

  @NonCPS
  def emailBody() {

        def TITLE = "Jenkins Prod Slave Status"
        def BODY = null
        jenkins = Hudson.instance
        def jenSlaves = "<slave names>" //  add slave names here with (,)comma separated

            def jenSlave = jenSlaves.split(' ')
            def numberOfflineNodes = 0
            def errorStr = ""
            for (slaveStr in jenSlave) {
            for (slave in jenkins.slaves) {
                def computer = slave.computer
                if (slaveStr.trim() == computer.name) {
                def isOK = (!computer.offline && !slave.getComputer().isOffline() && !slave.getComputer().isTemporarilyOffline())
                if (!isOK) {
                    numberOfflineNodes ++
                    if (errorStr != "") {
                    errorStr = errorStr +"\n"+"${computer.name} is <b style='color:red'>Offline</b>"
                    } else {
                    errorStr = "${computer.name} is <b style='color:red'>Offline</b>"
                    }
                }
                }
            }
            }
            println ("Number of Offline Nodes: " + numberOfflineNodes)
            println ( errorStr )


        if ( numberOfflineNodes >0 )
        {
            def encodedUrl = "teams or slack url" // add MS teams or Slack URL here to notification
            BODY = "<pre>${errorStr}</pre>"

            def post = new URL(encodedUrl).openConnection()
            def message = "{\"title\": \"${TITLE}\", \"text\": \"${BODY}\", \"themeColor\": \"EA4300\"}"
            post.setRequestMethod("POST")
            post.setDoOutput(true)
            post.setRequestProperty("Content-Type", "application/json")
            post.getOutputStream().write(message.getBytes("UTF-8"));
            def postRC = post.getResponseCode();
            println(postRC);

             mail (
                body: BODY,
                charset: 'UTF-8',
                from: 'jenkins mail', // add from email address here
                mimeType: 'text/html',
                subject: TITLE,
                to: "to address" // add to email address here
             )
        }


  }

emailBody();
