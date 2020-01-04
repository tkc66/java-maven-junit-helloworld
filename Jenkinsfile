pipeline {
    options{
        timeout(time:1,unit:'HOURS')
    }
    environment {
        docker_image_name = "java8-maven3-junit5"
    }
    
    agent {
    dockerfile {
        additionalBuildArgs '--no-cache=true --build-arg "JENKINS_USER_ID=112" --build-arg "JENKINS_GROUP_ID=117"'
        args '-v ${PWD}/.m2:/usr/share/maven/.m2'
        dir '.'
        filename 'Dockerfile'
        label env.docker_image_name
        }
    }
    stages {
        stage('maven execution') {
            steps {
                script {
                    dir('.') {
                        sh 'set HTTP_PROXY=$HTTP_PROXY'
                        sh 'set HTTPS_PROXY=$HTTP_PROXY'
                        sh 'mvn clean package site'
                    }
                }
            }
        }
        stage('Analysis') {
            steps {
                script {
                    dir('.') {
//                         step([$class: 'JacocoPublisher',
//                               execPattern: 'target/*.exec',
//                               classPattern: 'target/classes',
//                               sourcePattern: 'src/main/java',
//                               exclusionPattern: 'src/test*'
//                         ])
                        // step([$class: 'CoberturaPublisher',
                        //   coberturaReportFile: '**/reports/coverage.xml',
                        //   failUnhealthy: false,
                        //   failUnstable: false,
                        //   maxNumberOfBuilds: 0,
                        //   sourceEncoding: 'UTF_8'
                        // ])
                        // stepcounter settings: [[encoding: 'UTF-8', filePattern: 'web/**/*.py', filePatternExclude: 'web/tests/**/*.py,web/migrations/**/*.py,web/test_*.py', key: 'SourceCode'],[encoding: 'UTF-8', filePattern: 'web/tests/**/*.py,web/test_*.py', key: 'TestCode']]
                        // junit '**/reports/junit.xml'
                         sh 'echo "Analysis stage"'
                         checkstyle canComputeNew: false, canRunOnFailed: true, defaultEncoding: '', healthy: '', pattern: '', unHealthy: ''
                         findbugs canComputeNew: false, canRunOnFailed: true, defaultEncoding: '', excludePattern: '', healthy: '', includePattern: '', pattern: '', unHealthy: ''
                         recordIssues(
                              enabledForFailure: true, aggregatingResults: true,
                              tools: [java(), checkStyle(pattern: 'checkstyle.xml', reportEncoding: 'UTF-8'),
                              spotBugs(pattern: 'findbugs-exclude.xml', reportEncoding: 'UTF-8')]
                         )
                         stepcounter settings: [[encoding: 'utf-8', filePattern: 'src/main/', filePatternExclude: '', key: 'main'], [encoding: 'utf-8', filePattern: 'src/test/', filePatternExclude: '', key: 'test']]

                    }
                }
            }
        }
    }

}
