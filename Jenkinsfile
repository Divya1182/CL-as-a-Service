@Library('conduit@master') _

env.TERRAFORM_VERSION = "1.0.0"

String containerImage = 'registry.cigna.com/enterprise-devops/aws-d-megatainer'
String containerVersion = '1.0.9'

cignaBuildFlow {
    gitlabConnectionName = 'gitlab_server'
    cloudName = 'terraform-community-openshift-devops1'
    phases = [
        [
            lintingTypes: [
                'plz' : [
                    containerImage: containerImage,
                    containverVersion: containerVersion,
                    runLintAsTest: true,
                    verbosityFlag: '-vvv',
                ]
            ],
            branchPattern: '.*'
        ],
        [
            buildType: 'Plz',
            branchPattern: '.*',
            stashEnabled: false,
            containerImage: containerImage,
            containerMemory: 5000,
            containerVersion: containerVersion,
            sdlcEnvironment: 'ci',
            verbosityFlag: '-vvv',
            awsFed: [
                credentialsId: 'sa-jenkins-non-prd',
            ],
            tagDetails: [
                branch: 'master',
                gitTagCredKey: 'tf-modules-pat',
                tagFile: './module.version',
            ],
            sonarQube: [
                credentialsId: 'SonarAPIKey',
                scannerProperties: [
                    'sonar.projectKey=aws-s3',
                    'sonar.projectName=aws-s3',
                    'sonar.sources=./test',
                    'sonar.exclusions=plz-out/',
                    'sonar.inclusions=*.tf',
                    'sonar.mainBranch=master',
                    'sonar.analysis.ciid=CIThisisALib'
                 ]
            ],
            veracode: [
                credentialsId: 'veracode-service-id',
                zipUploadIncludesPattern: true,
                settings: [
                    applicationName: 'aws-s3',
                    canFailJob: false,
                    criticality: 'VeryHigh',
                    fileNamePattern: '*.tf',
                    replacementPattern: '',
                    scanExcludesPattern: 'plz-out/**',
                    scanIncludesPattern: '',
                    scanName: 's3-scan',
                    teams: 'dna',
                    uploadExcludesPattern: 'plz-out/**',
                    uploadIncludesPattern: '*.tf',
                    debug: true
                ]
            ]
        ]
    ]
}