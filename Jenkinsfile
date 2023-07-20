pipeline {
    agent any

    stages {
        stage ('Image building') {
            steps {
                script {
                    //dockerapp = docker.build("cristianomartinsdiasspbr1982/temperature-converter:v${env.BUILD_ID}", '-f ./APITemperaturas/Dockerfile ./APITemperaturas')
                    dockerapp = docker.build("cristianomartinsdiasspbr1982/temperature-converter:latest", '-f ./APITemperaturas/Dockerfile ./APITemperaturas')
                }
            }
        }

        stage ('Docker Hub Image pushing') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                        dockerapp.push('latest')
                        //dockerapp.push("v${env.BUILD_ID}")
                    }
                }
            }
        }

        stage ('Kubernetes deployment') {
            environment {
                tag_version = "${env.BUILD_ID}"
            }
            steps {
                script {
                    withKubeConfig([credentialsId: 'kubeconfig']) {
                    //withKubeConfig() {
                        //sh 'sed -i "s/{{tag}}/$tag_version/g" ./APITemperaturas/k8s/deployment.yaml'
                        sh 'kubectl apply -f ./APITemperaturas/k8s/deployment.yaml'
                    }
                }
            }
        }
    }
}

// pipeline {
//     agent any
    
//     stages {
//         stage('Checkout') {
//             steps {
//                 git 'https://github.com/cristianomartinsdias82/TemperatureConverter_JenkinsCI-CD.git'
//             }
//         }
        
//         stage('Restore') {
//             steps {
//                 // Restoring NuGet packages
//                 bat 'dotnet restore'
//             }
//         }
        
//         stage('Build') {
//             steps {
//                 // Building the .NET application
//                 bat 'dotnet build -c Release'
//             }
//         }
        
//         stage('Test') {
//             steps {
//                 // Running tests for the ASP.NET Web API application
//                 bat 'dotnet test --logger "trx;LogFileName=test-results.trx" --results-directory ./TestResults'

//                 //dotnet test --collect "XPlat Code Coverage"
//                 //reportgenerator -reports:./TestResults/**/coverage.cobertura.xml --reportTypes:Html -targetDir:./TestResults/
                
//                 // Optionally, you can publish the test results to Jenkins
//                 step([$class: 'MSTestPublisher', testResultsFile:"**/TestResults/*.trx"])
//             }
//         }
        
//         stage('Publish') {
//             steps {
//                 // Publishing the ASP.NET Web API application
//                 bat 'dotnet publish -c Release -o ./publish'
//             }
//         }
//     }
    
//     post {
//         success {
//             // If the build and tests are successful, you can deploy the application to your desired location here.
//             echo 'Build and test successful! Deploying...'
//             // Add your deployment steps here
//         }
//         failure {
//             echo 'Build or test failed! Deployment aborted.'
//             // Optionally, you can trigger notifications or other actions upon failure
//         }
//     }
// }