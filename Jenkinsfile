pipeline{
  agent any
    tools{
        maven 'Maven'
        }
        stages{
          stage('SonarQube scan'){
            steps{
              bat 'mvn sonar:sonar \
              -Dsonar.projectKey=another-java-maven-app \
              -Dsonar.host.url=http://localhost:9000 \
              -Dsonar.login=0e356f968180ba03c3f7f4ab1a90f7a29ae081c2'
              }
          }
          stage('Build'){
            steps{
              bat 'mvn -B -DskipTest clean package'
              }
            }
          stage('Test'){
            steps{
              bat 'mvn test'
              }
            post{
              always{
                junit 'target/surefire-reports/*.xml'
                }
              }
            }
          stage('Publish to Artifactory'){
            steps{
              rtMavenDeployer(
                id:'deployer',
                serverId:'Pipeline_repository@artifactory',
                releaseRepo:'Pipeline_repository',
                snapshotRepo:'Pipeline_repository'
              )
              rtMavenRun(
                pom:'pom.xml',
                goals:'clean install',
                deployerId:'deployer'
              )
              rtPublishBuildInfo(
                serverId:'Pipeline_repository@artifactory'
              )
             }
          }
        }
      }

            

