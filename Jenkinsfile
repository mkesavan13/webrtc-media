#!groovy
library identifier: 'sparkPipeline', changelog: false

sparkPipeline {
  name = 'webrtc-media-core'
  services = []
  junitPattern = '**/junit.xml'
  numToKeep = 5
  notifySparkRoomId = 'Y2lzY29zcGFyazovL3VzL1JPT00vM2I2ZmY0MjAtZjI2YS0xMWViLWI1MWMtZmZjMTg3YWY3Yjgw'
  builder = 'NODE_JS_BUILDER'

  build = { services ->
    this.sh './pipeline/setup.sh'
    this.sh './pipeline/unit-test.sh'

    /*
      Commented out Integ tests as we face some issue with sauce labs
      Created a followup ticket for the same -> https://jira-eng-gpk2.cisco.com/jira/browse/SPARK-308848
      this.withCredentials([
        this.string(credentialsId: 'SAUCE_TOKEN', variable: 'SAUCE_ACCESS_KEY')
      ]) {
        this.sh './pipeline/integration-test.sh'
      }
    */

    if(this.isMasterBranch()) {

      this.sh './pipeline/build.sh'

      this.withCredentials([
        this.string(credentialsId: 'GITHUB_TOKEN', variable: 'GITHUB_TOKEN'),
        this.string(credentialsId: 'ARTIFACTORY_TOKEN', variable: 'ARTIFACTORY_TOKEN')
      ]){
        this.sh './pipeline/release.sh'
      }

      this.sh './pipeline/cleanup.sh'

      integration.deployMode = 'skip'
      production.deployMode = 'skip'
    }
  }
}
