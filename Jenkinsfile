//Variable to store the details of docker image after building it to use in push stage
builtImg = ''

pipeline {
    agent any
    //options {
    //skipDefaultCheckout true
    //}
    environment {
        registry = "docker_hub_account/repository_name"
        registryCredential = '9d5388a6-334d-4121-9138-d191463912ec'
    }
    parameters {
        string(name: 'projectName', defaultValue: 'sph-golden-images', description: 'Name of the project')
        //string(name: 'sourceCodeRepo', defaultValue: 'https://github.com/starlord-dixon/sph-golden-images.git', description: 'Source Code Repository')
        //string(name: 'buildBranch', defaultValue: 'master', description: 'Which branch should be built , this could be the parent or the feature branch')
        string(name: 'gitRepoCredentials', defaultValue: '5398079d-724f-4c2e-a291-4be6dc7f922d', description: 'Build Script Branch')
        string(name: 'registryUrl', defaultValue: 'https://registry-1.docker.io/v2', description: 'Container Registry URL')
        string(name: 'dockerRepository', defaultValue: 'sph-digital/debian-golden-image', description: 'The docker repository')
        string(name: 'registryCredentialsId', defaultValue: 'docker-registry-creds', description: 'The credentials id that points to the Container Registry credentials')
    }
    triggers {
        GenericTrigger(
            genericVariables: [
                [key: 'ref', value: '$.ref'],
                [key: 'changed_files', value: '$.commits[*].[\'modified\',\'added\',\'removed\'][*]']
            ],
            causeString: 'Triggering build for files $changed_files on $ref',
            token: 'react-fpm',
            printContributedVariables: false,
            printPostContent: false,
            //regexpFilterText: '$ref $changed_files',
            //regexpFilterExpression: 'refs/heads/' + buildBranch + '\\s((.*"(debian/)[^"]+?".))'
        )
    }
    stages{
        /*stage("Prepare SCM"){
            steps{
                script{
                    //cleanWs()
                    git branch: params.buildBranch, url: params.sourceCodeRepo, credentialsId: params.gitRepoCredentials
                    echo "Removing git references"
                    sh "rm -rf .git"
                    echo 'Prepared'
                }
            }
            post {
                failure {
                    echo 'I failed :('
                }
            }
        }*/
        stage("Build Docker Image"){
            steps{
                script{
                        echo 'Build docker image'
                        dockerfile = "Dockerfile"
                        builtImg = docker.build("${params.dockerRepository}", "-f ${dockerfile} .")
                }
            }
            post {
                failure {
                    echo 'I failed :('
                }
            }
        }
        stage("Push to Registry"){
            steps{
                script{
                    echo 'Pushing to Repo'
                    // docker.withRegistry("${params.registryUrl}", "${params.registryCredentialsId}" ){
                    //     builtImg.push("v${BUILD_NUMBER}");
                    // }
                }
            }
            post {
                failure {
                    echo 'I failed :('
                        sh "docker images | grep ${params.dockerRepository} | awk '{print \$3}' | xargs docker rmi -f"
                        echo "${projectName} - Cleaning workspace"
                }
            }
        }
    }
    post { 
        always {
            cleanWs()
        }
    }
}
