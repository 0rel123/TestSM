def cmd(command) {
    if (isUnix()) { sh "${command}" } else { bat "chcp 65001\n${command}"}
}

def connectionString

pipeline {
     agent {
        label 'BDD'
    }
    stages {
       
        stage('Smoke') {
            steps {
                timestamps {								
                    cmd("runner xunit ./Tests/Smoke --reportsxunit ГенераторОтчетаJUnitXML{out/xunit/junit.xml};ГенераторОтчетаAllureXML{out/xunit/allure.xml} --xddConfig ./tools/JSON/xunitfor1C.json --ibconnection \"${env.connectionString}\"  --db-user ${Base1C_Usr} --db-pwd  ${Base1C_Psw} --testclient ${Base1C_Usr}:${Base1C_Psw}:1538 --xddExitCodePath out/BuildStatus.log")
    			}
            }
        }


    }

    post { 
        always { 
            allure includeProperties: false, jdk: '', results: [[path: 'out/vanessa'], [path: 'out/xunit']]
            junit allowEmptyResults: true, testResults: 'out/junit.xml, out/syntax-check/junit.xml' 
            emailext body: '''$PROJECT_NAME - Build # $BUILD_NUMBER - $BUILD_STATUS: Check console output at $BUILD_URL to view the results.''', subject: '$PROJECT_NAME - Build # $BUILD_NUMBER - $BUILD_STATUS!', to: 'AZharkov@sportmaster.ru; DKazakov@sportmaster.ru;'
        }
    }

}
