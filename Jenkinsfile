def matchedJobs = Jenkins.instance.items.findAll { job ->
    job.name =~ /.*_staging/
}

matchedJobs.each {
  prop = it.getProperty(ParametersDefinitionProperty.class)
  if(prop != null) {
    repo_name = it.fullName.replace('_staging', '')
    (start_point, repo_version) = getRepoVersion(repo_name)
    dev_branch = getDevBranch(repo_name)
    if (repo_version != null) {
       params = []
       println "${repo_version} ${dev_branch} ${start_point}"
       params.push(new StringParameterValue('VERSION', repo_version))
       params.push(new StringParameterValue('DEV_BRANCHES', dev_branch))
       params.push(new StringParameterValue('START_POINT', start_point))
       it.scheduleBuild(0, new Cause.UserIdCause(), new ParametersAction(params))
    }
  }
}

def getRepoVersion(repo) {
    def proc = "git ls-remote --tags infra:${repo} ${repo}-v*".execute() | "grep -o refs/tags/${repo}-v[0-9]*.[0-9]*.[0-9]*".execute() | "sort -rV".execute()
    version = proc.text.readLines().collect {it.replaceAll('refs/tags/', '')}[0]
    (major, minor, patch) = version.replace("${repo}-v", '').tokenize('.')
    patch = patch.toInteger() + 1
    return [version, "${major}.${minor}.${patch}"]
}

def getDevBranch(repo) {  
    def gettags = ("git ls-remote infra:${repo} feature/dev").execute()
    return gettags.text.readLines().collect {it.split()[0]}[0]
}


