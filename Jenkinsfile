//Variable to store the details of docker image after building it to use in push stage
builtImg = ''

pipeline {
    agent any
    options {
    skipDefaultCheckout true
    }
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
        
        stage("Check CIS compliance"){
            steps{
                script{
                    ansiColor('xterm') {
                        sh 'docker run --net host --pid host --userns host --cap-add audit_control -e DOCKER_CONTENT_TRUST=$DOCKER_CONTENT_TRUST -v /etc:/etc:ro -v /usr/bin/docker-containerd:/usr/bin/docker-containerd:ro -v /usr/bin/docker-runc:/usr/bin/docker-runc:ro -v /usr/lib/systemd:/usr/lib/systemd:ro -v /var/lib:/var/lib:ro -v /var/run/docker.sock:/var/run/docker.sock:ro --label docker_bench_security docker/docker-bench-security -c check_4,check_4_1,check_4_2,check_4_3,check_4_4,check_4_5,check_4_6,check_4_7,check_4_8,check_4_9,check_4_10,check_4_11,check_4_end -i *sph-digital/debian-golden-image*'
                    }
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
