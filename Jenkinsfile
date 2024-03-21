                                                  credentialsId: 'githubPAT', 
                                                  name: 'main']]])

                    // Commit hash after checkout
                    def afterCheckout = sh(script: 'git rev-parse HEAD', returnStdout: true).trim()

                    if (beforeCheckout != afterCheckout) {
                        echo 'New commits found. Proceeding with the pipeline.'
                    } else {
                        echo 'No new commits. Aborting the pipeline.'
                        error('No new commits. Aborting the pipeline.') 
                    }
                }
            }
        }

        stage('Execute Fastlane Script') {
            steps {
                script {
                    env.FASTLANE_OPT_OUT_USAGE = 'true'

                    checkout([$class: 'GitSCM', 
                              branches: [[name: 'main']], 
                              userRemoteConfigs: [[url: 'https://github.com/PetFinder2/jenkins.git', 
                                                  credentialsId: 'githubPAT', 
                                                  name: 'main']]])

                    dir('Starlight/app/ios/fastlane') {
                        // Install Bundler
                        sh 'gem install bundler'

                        // Use Bundler to install gems
                        sh 'bundle install'

                        // Execute Fastlane command with bundle exec
                        sh 'bundle exec fastlane ios print_hello'
                    }
                }
            }
        }
    }
}