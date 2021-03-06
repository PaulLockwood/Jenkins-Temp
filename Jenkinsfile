pipeline {
  agent {
    // "node" is a new agent type that works the same as "label" but allows 
    // additional parameters, such as "customWorkspace" below.
    node {
      label "master"
      // This is equivalent to the "ws(...)" step - sets the workspace on the
      // agent to a hard-coded path. If it's not an absolute path, it'll be
      // relative to the agent's workspace root.
      customWorkspace "xyzzy-custom-ws"
    }
  }
  
  stages {
    stage('some-stage') {
      // REGRESSION: JENKINS-42762 - in 1.1.1, you can't nest multiple 
      // conditions directly within "when". You need to use "when { allOf { ... } }"
      // instead. This will be fixed in the next release.
      when {
        // We have added logical conditions for "when" - specifically...

        // "anyOf" - contains 1 or more nested condition and will resolve
        // to true if *any* of the nested conditions are true.
        anyOf {
          // "allOf" - contains 1 or more nested conditions an will resolve
          // to true if *all* of the nested conditions are true.
          allOf {
            branch "master"
            expression {
              // REGRESSION: JENKINS-42829 - methods defined elsewhere in
              // the Jenkinsfile currently can't be used in "when" "expression"s.
              return true
            }
          }
          // "not" - contains 1 nested condition and will resolve to true
          // if the nested condition is false.
          not {
            branch "some-other-branch-entirely"
          }
        }
      }
      steps {
        // We've added the "validateDeclarativePipeline('some-file')" step,
        // which will let you perform validation of a Declarative Pipeline
        // from within a different Pipeline. This step can be used in 
        // Scripted Pipeline as well.
        echo validateDeclarativePipeline("some-other-file.groovy")
      }
    }
  }

  
}