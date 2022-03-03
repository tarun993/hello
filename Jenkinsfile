pipeline {
    agent any
    
 

    options {
        buildDiscarder(logRotator(numToKeepStr: '15'))
        disableConcurrentBuilds()
    }
    
  
    parameters{
        booleanParam(defaultValue:false, name: 'EXECUTE_PULL')
        booleanParam(defaultValue:false, name: 'EXECUTE_PUSH')
        string(name: 'PLANET', defaultValue: 'Earth', description: 'Which planet are we on?')
        string(name: 'GREETING', defaultValue: 'Hello', description: 'How shall we greet?')
        
    }
        
    triggers {
        parameterizedCron('''
            */2 * * * * %EXECUTE_PULL=true
            */3 * * * * %EXECUTE_PUSH=true
        ''')
        
    }
    stages {
        
        stage('echo') {
            steps{
                echo "class of EXECUTE_PULL is ${EXECUTE_PULL.getClass()}"   
            echo "EXECUTE_PULL=${EXECUTE_PULL}"
            echo "EXECUTE_PUSH=${EXECUTE_PUSH}"
            echo "${PLANET}"
            echo "${GREETING}"
            }
        }
        
        stage("Wait") {
            steps {
        
                sleep time: 5, unit: 'SECONDS' 
            }
        }
        
        
        
        
        
        stage('Hello_pull') {
            
            when {expression {EXECUTE_PULL == "true"}}
            
            steps {
                echo 'Hello World pull'
            }
        }
        
        stage('check_stage'){
            when {expression {EXECUTE_PUSH == "true"}}
                steps{
                    script{     
                powershell '''
                write-host("${EXECUTE_PUSH -eq "true")}")
                if($EXECUTE_PUSH -eq "true"){
                echo "${EXECUTE_PUSH}"
                $EXECUTE_PUSH = "false"
                echo "${EXECUTE_PUSH}"}'''
                    }
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
      post {
        // Clean after build
         always {
             cleanWs(cleanWhenNotBuilt: false,
                    deleteDirs: true,
                    disableDeferredWipeout: true,
                    notFailBuild: true,
                     cleanWhenAborted:true,
                     cleanWhenFailure:true,
            cleanWhenSuccess:true,
            cleanWhenUnstable: true,
                    patterns: [[pattern: '.gitignore', type: 'INCLUDE'],
                               [pattern: '.propsfile', type: 'EXCLUDE']])
        }
    }
 }
             
