---
title: Github for Coding Interviews
---

Many paid services exist to perform coding interviews, but they differ from actual coding environments. Instead of forcing job applicants into an unfamiliar service, why not use familiar tools to conduct the interview. An approach that works for me involves using Github to contribute a solution using their development environment. Another benefit is the simplicity of this approach.

**Caveat:** every company I've worked with has used and supported OSS software. If your company isn't as supportive, then this approach might not translate to your organization.

# Setup the coding interview question

1. Setup a private Github repo. (Free tier)
1. Work on whatever coding problem makes sense for your org.
1. Make this repository a Private Template by going into Settings.

# Adminster the coding interview to a candidate

Only two(2) commands are necessary to administer the coding interview using Github. You only need one piece of information from the candidate: their Github username.

```
> export GITHUB_USERNAME=applicantGithubUsername
> gh repo create --private --template myorg/coding-interview myorg/coding-interview-${GITHUB_USERNAME}
> gh api -XPUT repos/myorg/coding-interview-${GITHUB_USERNAME}/collaborators/${GITHUB_USERNAME}
```

# Additional benefits

Since Github doesn't charge for unlimited private repositories, you can maintain each candidate's environment and keep everything isolated. Separate repositories allow the candidate to create Issues and Pull Requests and use any other features they'd like.

I've seen other companies use a similar approach, but they use a shared repository. The risk of a shared repo is that candidates can't (or shouldn't) create Pull Requests without leaking previous solutions to future applicants due to Github's design. Repository owners can never delete Pull Requests.