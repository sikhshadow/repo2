pipeline {
    agent none

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "mymaven"
        jdk "myjava"
            }

    stages {
        stage('compile') {
            agent any
            steps {
                // Get some code from a GitHub repository
                git 'https://github.com/sikhshadow/repo2.git'

                // Run Maven on a Unix agent.
                sh "mvn compile"

                // To run Maven on a Windows agent, use
                // bat "mvn -Dmaven.test.failure.ignore=true clean package"
                  }

                         }
             stage('codereview') {
            agent any
            steps {
              sh "mvn pmd:pmd" 
            }
             }
             stage('UnitTest') {
            agent any
            steps {
              sh "mvn test" 
                    }
            post 
             { always 
                {
                junit 'target/surefire-reports/*.xml'
                }
              }
             }
            
             
             stage('MetricCheck') {
            agent any
            steps {
              sh "mvn cobertura:cobertura -Dcobertura.report.format=xml" 
            }
            post {
                always {
                cobertura coberturaReportFile: 'target/site/cobertura/coverage.xml'
                        }
                 }
             }
            stage('Package') {
            agent {label 'Linux_Slave_1'}
            steps {
                git 'https://github.com/sikhshadow/repo2.git'
                sh "mvn package" 
            }
            }
        }
    }

