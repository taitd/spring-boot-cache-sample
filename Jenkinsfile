echo "Starting my workflow"

stage 'build'
node {

    echo "Download fonts"
    git 'https://github.com/taitd/spring-boot-cache-sample.git'

    echo "Building the project with Gradle Wrapper"
    sh './gradlew build -x test'

    archive 'build/libs/*.jar'

}

stage 'test'
node {
    sh './gradlew clean test'

    step([$class: 'JUnitResultArchiver', testResults: '**/build/test-results/TEST-*.xml'])
}

stage 'integration-test'
node {
    def outcome = input message: 'Do you want to run integration tests?', parameters: [[$class: 'BooleanParameterDefinition', defaultValue: false, description: '', name: '']]
    if (outcome) {
        sh './gradlew integrationTest'
    }
}
