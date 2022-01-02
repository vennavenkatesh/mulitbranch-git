pipeline {
    agent any
    
    parameters {
        string(name: 'IP', description: 'Please enter you backend IP')
        //string(name: 'GIT', description: 'Please enter your Git url')
        gitParameter branchFilter: 'origin/(.*)', defaultValue: 'master', name: 'BRANCH', type: 'PT_BRANCH', useRepository: 'https://github.com/vennavenkatesh/devops-sample.git'
        //string(name: 'branch_name', description: 'Please enter your branch')
    }
    stages {
        stage("Git checkout") {
            steps {
                echo "checkout the code from bitbucket"
                git branch: "${params.branch_name}", url: "https://github.com/vennavenkatesh/devops-sample.git" , poll: true
            }
        }
        
        stage("sonar") {
            steps {
                echo "executing the test cases"
               // sh "cd $WORKSPACE && mvn sonar:sonar"
            }
        }
        

        stage("Test-cases & Build") {
            steps {
                echo "executing the test cases"
                sh "cd $WORKSPACE && mvn clean install"
            }
        }
        
         stage("Nexus-Deploy") {
            steps {
                echo "executing the test cases"
                sh "cd $WORKSPACE && mvn clean deploy"
            }
        }

        stage("Backup") {
            steps {
                    
                        echo "Print the workspave path"
                        echo "Hello ${params.NAME}"
                        sh "echo $WORKSPACE"
                        sh '''
                        #!/bin/bash
                        ssh tomcat@$IP << EOF
                        cd /opt/apache-tomcat-9.0.56/webapps
                        mv *.war /opt/apache-tomcat-9.0.56/backup
                        exit 0
                        EOF'''
                }
            }
        

        stage("Deploy-tomcat-server") {
            steps {
                echo "Deploy into tomcat server"
                sh "cd $WORKSPACE/target/"
                sh "cd $WORKSPACE/target/ && scp -r *.war tomcat@3.109.56.173:/opt/apache-tomcat-9.0.56/webapps/"
                
                echo " Deployment has been completed successfully"
            }    
        }
}
