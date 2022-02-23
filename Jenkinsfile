pipeline {
    agent any

    options {
        buildDiscarder(logRotator(numToKeepStr: '5'))
        
    }
    parameters {
        booleanParam(name: 'parameter_name', value: false)
        booleanParam(name: 'EXECUTE_PULL', value: false)
        booleanParam(name: 'EXECUTE_PUSH', value: false)
        
        
    }
    
    triggers {
        parameterizedCron('''
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
            
