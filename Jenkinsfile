pipeline {
    agent any
    
 

    options {
        timestamps()
        buildDiscarder(logRotator(numToKeepStr: '15'))
        disableConcurrentBuilds()
    }
    
  
    parameters{
        booleanParam(defaultValue: true, name: 'EXECUTE_PULL')
        booleanParam(defaultValue: true, name: 'EXECUTE_PUSH')
        string(name: 'PLANET', defaultValue: 'Earth', description: 'Which planet are we on?')
        string(name: 'GREETING', defaultValue: 'Hello', description: 'How shall we greet?')
        
    }
        
    triggers {
        parameterizedCron('''
            * * * * * %EXECUTE_PULL=true
            */3 * * * * %EXECUTE_PUSH=true
        ''')
        
    }
    stages {
        
        stage('echo') {
            steps{
            echo "EXECUTE_PULL=${EXECUTE_PULL}"
            echo "$EXECUTE_PUSH=${EXECUTE_PUSH}"
            echo "${PLANET}"
            echo "${GREETING}"
            }
        }
        
        stage("Wait") {
            steps {
                sleep time: 20, unit: 'SECONDS' 
            }
        }
        
        stage('Hello_pull') {
            
            when {expression {EXECUTE_PULL == "true"}}
            
            steps {
                echo 'Hello World pull'
            }
        }
        
        stage('check_stage'){
            when {expression {EXECUTE_PULL == "true"}}
            steps{
                
                powershell '''if(${EXECUTE_PUSH}){
$EXECUTE_PUSH=$false}'''
                
            }
        }  
        stage('Hello_push') {
                
            when {expression {EXECUTE_PUSH == "true"}}
            
            steps {
                echo "${EXECUTE_PUSH}"
                echo 'Hello World push'
            }
        }
     }        
 }
             
