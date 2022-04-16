pipeline {
    agent any
    
 

    options {
        buildDiscarder(logRotator(numToKeepStr: '15'))
        disableConcurrentBuilds()
    }
    
  
    parameters{
        booleanParam(defaultValue:true, name: 'EXECUTE_PULL')
        booleanParam(defaultValue:true, name: 'EXECUTE_PUSH')
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
        stage('Hello_pull') {
            
            when {expression {EXECUTE_PULL == "true"}}
            
            steps {
                echo 'Hello World pull'
            }
        }
        
        stage('check_stage'){
            when {expression {EXECUTE_PUSH == "true"}}
                steps{
                    script {      
                EXECUTE_PUSH = powershell '''
                write-host "{${env:EXECUTE_PUSH} -eq "true"} line63"
                if($True){
                return "false"}
                '''   
                        println "hello, ${EXECUTE_PUSH}"    }
                
            }
        }  
        stage('Hello_push') {
                
            when {expression {EXECUTE_PUSH == "true"}}
            
            steps {
                echo "\${EXECUTE_PUSH_1} is $EXECUTE_PUSH_1"
                echo "\${EXECUTE_PUSH} is ${EXECUTE_PUSH}"
                echo "\${env.EXECUTE_PUSH} is ${env.EXECUTE_PUSH}"
                echo "\${env:EXECUTE_PUSH} is ${env:EXECUTE_PUSH}"
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
             
