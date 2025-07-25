# GitLab Gemini Reviewer

An automated code review assistant powered by Google's Gemini AI. This tool analyzes GitLab Merge Requests, provides feedback as comments, creates Jira tickets, and generates QA test plans, streamlining your development workflow.

## ✨ Features

- **Automated Code Analysis**: Leverages the Gemini API to perform detailed code reviews.
- **Smart MR Discussions**: 
  - Tracks existing discussions to avoid duplicates
  - Automatically resolves discussions when issues are fixed
  - Filters discussions by severity before creating new ones
- **Inline MR Commenting**: Posts suggestions and issues as discussions directly on the relevant code lines in the merge request.
- **Overall Summary**: Adds a general summary comment to the MR with a quality score.
- **Automatic Approval & Merge**: Can automatically approve and optionally merge requests that meet a configurable quality threshold.
  - Control merging behavior with the `--auto-merge` flag or `AUTO_MERGE` environment variable
- **Seamless Jira Integration**:
  - Creates a specific sub-task for the code review under the main story.
  - Generates and posts a detailed QA test plan as a comment on the parent ticket.
- **Multi-language Support**: Can provide feedback in English or Brazilian Portuguese.
- **Highly Configurable**: All parameters are controlled via environment variables, making it perfect for CI/CD environments.

## 🚀 How It Works

1.  **Trigger**: The tool is designed to be run in a CI/CD pipeline when a merge request is created or updated.
2.  **Fetch Diffs**: It fetches the code changes (diffs) from the specified GitLab merge request.
3.  **Check Existing Discussions**: 
    - Fetches all existing discussions to avoid duplicates
    - Resolves discussions for issues that have been fixed
4.  **Analyze**: It sends the code, along with contextual information from the MR title and description, to the Gemini API for analysis.
5.  **Post Feedback**: It parses the AI's response and posts feedback to GitLab:
    - A general summary note.
    - Discussions attached to specific lines of code for each new issue found.
6.  **Integrate with Jira**:
    - Extracts the Jira ticket key from the MR title (e.g., `PROJ-123`).
    - Creates a "Code Review" sub-task.
    - Generates a complete QA test plan and posts it as a comment on the main Jira ticket.

## 📦 Installation

You can install the package directly from the source code.

```bash
# Clone the repository first
git clone https://github.com/alairjt/gitlab-gemini-reviewer.git
cd gitlab-gemini-reviewer

# Install using pip
pip install .
```

## ⚙️ Configuration

The tool is configured entirely through environment variables.

| Variable                 | Description                                                              | Default                  | Required |
| ------------------------ | ------------------------------------------------------------------------ | ------------------------ | :------: |
| `GITLAB_TOKEN`           | Your GitLab personal access token with `api` scope.                      | -                        |   Yes    |
| `GEMINI_API_KEY`         | Your Google AI Studio API key for Gemini.                                | -                        |   Yes    |
| `CI_PROJECT_ID`          | The ID of your GitLab project. Provided by GitLab CI.                    | -                        |   Yes    |
| `CI_MERGE_REQUEST_IID`   | The IID (internal ID) of the merge request. Provided by GitLab CI.       | -                        |   Yes    |
| `CI_SERVER_URL`          | The base URL of your GitLab instance (e.g., `https://gitlab.com`).        | -                        |   Yes    |
| `JIRA_URL`               | The base URL of your Jira instance.                                      | -                        |    No    |
| `JIRA_USER`              | The email or username for the Jira service account.                      | -                        |    No    |
| `JIRA_TOKEN`             | The API token for the Jira service account.                              | -                        |    No    |
| `REVIEW_LANGUAGE`        | The language for the AI's response (`en` or `pt-BR`).                    | `pt-BR`                  |    No    |
| `GEMINI_MODEL`           | The Gemini model to use for the review.                                  | `gemini-1.5-flash`       |    No    |
| `DEBUG`                  | Set to `1` or `true` for verbose error logging.                          | -                        |    No    |

**Note**: The Jira variables are only required if you want to enable Jira integration.

## ▶️ Usage

After installation, the tool can be run from the command line.

```bash
gemini-reviewer
```

If you encounter a `command not found` error, it means the installation directory is not in your shell's `PATH`. You can either add it or run the tool as a module:

```bash
python -m gitlab_gemini_reviewer.gemini_mr_review
```

### GitLab CI/CD Example

It's intended to be used as a step in your CI/CD pipeline.

```yaml
# .gitlab-ci.yml

gitlab_gemini_reviewer:
  stage: review
  image: python:3.11-slim
  before_script:
    - pip install --upgrade pip
    - pip install gitlab-gemini-reviewer>=0.3.0
  script:
    - gemini-reviewer
  needs: ['unit test'] # ✅ Ensures that the 'unit test' stage runs before.
  rules:
    # Main rule:
    # 1. Run only on Merge Request events.
    # 2. Do NOT run if the target branch is 'production' or 'pre-production'.
    - if: '$CI_PIPELINE_SOURCE == "merge_request_event" && $CI_MERGE_REQUEST_TARGET_BRANCH_NAME != "production" && $CI_MERGE_REQUEST_TARGET_BRANCH_NAME != "pre-production"'
      when: on_success # Runs if the job in 'needs' succeeds (default).
  interruptible: true # ✅ Cancels the old job if a new commit is pushed to the MR, saving resources.


```

## 🔄 Discussion Management

The tool now includes intelligent discussion management features:

### Avoiding Duplicate Discussions
- Automatically checks for existing discussions on the same lines of code
- Prevents creating duplicate discussions for the same issue
- Uses file path and line number to identify duplicate issues

### Automatic Resolution
- Tracks when issues are fixed in subsequent commits
- Automatically resolves discussions when the corresponding issues are no longer present
- Only creates new discussions for issues that don't already have open discussions

### Severity Filtering
- Filters issues by severity before creating discussions
- Helps focus on the most important feedback first
- Configurable through the API response

## ⚙️ Auto-Merge Configuration

You can enable automatic merging of approved MRs using either the command line flag or environment variable:

```bash
# Using command line flag
gemini-reviewer --auto-merge

# Using environment variable
export AUTO_MERGE=true
gemini-reviewer
```

### Important Notes:
- The merge will only be attempted if the review is approved
- The user running the tool must have merge permissions on the target repository
- If the merge fails (e.g., due to merge conflicts), a warning will be logged

## 📄 License

This project is licensed under the MIT License.