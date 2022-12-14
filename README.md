# CI Utils for WongNung
This repository is a collection of small(ish) utilities for the [WongNung/WongNung](https://github.com/WongNung/WongNung) repository.

All the tools are written based on GitHub-hosted runners. You can check their runner specification [here](https://docs.github.com/en/actions/using-github-hosted-runners/about-github-hosted-runners#supported-runners-and-hardware-resources).

Most of the utilities run on `ubuntu-latest` image. Any other images might need modifications to utilities, feel free to fork and do that.

# Setup
In your GitHub Actions workflow, include a step to checkout this repository.

```yaml
name: Checkout ci-utils
uses: actions/checkout@v3
with:
  repository: WongNung/ci-utils
  path: "/ci"
```

Then, you can copy any utility from this repository to workflow. Example:

```yaml
name: Lint and generate summary
run: |
  cp /ci/flake8-summarize .
  ./flake8-summarize >> $GITHUB_STEP_SUMMARY
```

# Utilities
* [flake8-summarize](flake8-summarize): a utility made specifically for GitHub Actions Summary feature, in Actions you should redirect its output to `$GITHUB_STEP_SUMMARY`
  
  **Example:**

  ```sh
  flake8-summarize >> $GITHUB_STEP_SUMMARY
  ```

* [test-dockerfile](test-dockerfile): a utility that will download [`container-structure-test`](https://github.com/GoogleContainerTools/container-structure-test) to workflow, and you can test your Dockerfile with it.

  **Prerequisites:**
  * `Dockerfile` written and is in repository
  * `cst-config.yml` written and is in repository, see syntax for `container-structure-test` YAML file in their README.md

  **To pass in Environment Variables,** you should add a section marker `# buildargs` to your Dockerfile
  ```dockerfile
  FROM ubuntu:latest
  #...
  
  # buildargs
  
  #...
  RUN echo "Hello world!"
  ```

  **Example:**
  ```sh
  # to use test-dockerfile, you need to specify image tag as an argument.
  test-dockerfile wongnung:testdrive
  
  # you can also pass in environment variables, using their names.
  # but you also need to specify a section for build arguements in Dockerfile
  export KEY=1234
  test-dockerfile wongnung:testdrive KEY
  ```

### Incompleted Utilities

* selenium-browser-neutralize: a utility that replaces all occurence of Python source files that are Selenium browser names to the same names, as set in executable argument.

  **Example:**
  ```sh
  # Changes all occurences of Firefox, Chromium, Opera etc. to Chrome
  selenium-browser-neutralize "Chrome"
  ```
