pipeline {
    agent any

    options {
        buildDiscarder(logRotator(numToKeepStr: '5'))
        
    }
    parameters {
        booleanParam(name: 'EXECUTE_PULL', defaultValue: false)
        booleanParam(name: 'EXECUTE_PUSH', defaultValue: false)
        
        
    }
    
    triggers {
        cron('''
            * * * * * %EXECUTE_PULL = true
            */2 * * * * %EXECUTE_PUSH = true
        ''')
        
    }
    stages {
        
        stage('Hello_pull') {
            
            when {expression {EXECUTE_PULL == "true"}}
            
            steps {
                echo 'Hello World pull'
            }
        }
        stage('Hello_push') {
                
            when {expression {EXECUTE_PUSH == "true"}}
            
            steps {
                echo 'Hello World push'
            }
         }
     }        
 }
            
