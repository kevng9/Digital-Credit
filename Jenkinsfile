pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building..'
            }
            {
                sh "mvn clean package -DbuildNumber=${env.BUILD_NUMBER} -DrunUnitTests=true"
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
                cifsPublisher(publishers: [[configName: 'Automic', transfers: [[cleanRemote: false, excludes: '', flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: 'target/', sourceFiles: 'target/*.jar']], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false]])
                createDepPckg appName: 'DIGITALCREDIT', folder: 'DEFAULT', owner: '100/AUTOMIC/AUTOMIC', pass: 'AUTOMIC', pkgName: '$BUILD_NUMBER', pkgType: 'Deployment', server: 'http://10.0.0.228:80/cda', useCentlCrd: true, user: '100/AUTOMIC/AUTOMIC'
                execApplnWkf appName: 'DIGITALCREDIT', executeAt: '', installationMode: 'overwrite', manualConfirmation: 'no', pass: 'AUTOMIC', pkgName: '$BUILD_NUMBER', profile: 'DEV', queue: '', server: 'http://10.0.0.228:80/cda', startAt: 'now', useCentlCrd: true, user: '100/AUTOMIC/AUTOMIC', userGroup: '', workflow: 'DeployDigitalCredit'
                sendNotificationToCDD appName: 'Digital-Credit', appVersion: 'V1', releaseTokens: '{"DCreditBuildNumber":"$BUILD_NUMBER"}'
            }
        }
    }
}
