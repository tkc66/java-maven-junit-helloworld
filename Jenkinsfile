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
                        step([$class: 'JacocoPublisher',
                              execPattern: 'target/*.exec',
                              classPattern: 'target/classes',
                              sourcePattern: 'src/main/java',
                              exclusionPattern: 'src/test*'
                        ])
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
//                        checkStyle canComputeNew: false, canRunOnFailed: true, defaultEncoding: '', healthy: '', pattern: '', unHealthy: ''
                        publishers {
                            checkstyle('**/checkstyle.xml') {
                                healthLimits(3, 20)
                                thresholdLimit('high')
                                defaultEncoding('UTF-8')
                                canRunOnFailed(true)
                                useStableBuildAsReference(true)
                                useDeltaValues(true)
                                computeNew(true)
                                shouldDetectModules(true)
                                thresholds(
                                        unstableTotal: [all: 1, high: 2, normal: 3, low: 4],
                                        failedTotal: [all: 5, high: 6, normal: 7, low: 8],
                                        unstableNew: [all: 9, high: 10, normal: 11, low: 12],
                                        failedNew: [all: 13, high: 14, normal: 15, low: 16]
                                )
                            }

                            findbugs('**/findbugs-exclude.xml', false) {
                                healthLimits(3, 20)
                                thresholdLimit('high')
                                defaultEncoding('UTF-8')
                                canRunOnFailed(true)
                                useStableBuildAsReference(true)
                                useDeltaValues(true)
                                computeNew(true)
                                shouldDetectModules(true)
                                thresholds(
                                        unstableTotal: [all: 1, high: 2, normal: 3, low: 4],
                                        failedTotal: [all: 5, high: 6, normal: 7, low: 8],
                                        unstableNew: [all: 9, high: 10, normal: 11, low: 12],
                                        failedNew: [all: 13, high: 14, normal: 15, low: 16]
                                )
                            }
                        }
//                        spotBugs canComputeNew: false, canRunOnFailed: true, defaultEncoding: '', excludePattern: '', healthy: '', includePattern: '', pattern: '', unHealthy: ''
//                        recordIssues(tools: [acuCobol()])

                    }
                }
            }
        }
    }

}
