# GitLab Gemini Reviewer

An automated code review assistant powered by Google's Gemini AI. This tool analyzes GitLab Merge Requests, provides feedback as comments, creates Jira tickets, and generates QA test plans, streamlining your development workflow.

## ✨ Features

- **Automated Code Analysis**: Leverages the Gemini API to perform detailed code reviews.
- **Inline MR Commenting**: Posts suggestions and issues as discussions directly on the relevant code lines in the merge request.
- **Overall Summary**: Adds a general summary comment to the MR with a quality score.
- **Automatic Approval**: Can automatically approve merge requests that meet a configurable quality threshold.
- **Seamless Jira Integration**:
  - Creates a specific sub-task for the code review under the main story.
  - Generates and posts a detailed QA test plan as a comment on the parent ticket.
- **Multi-language Support**: Can provide feedback in English or Brazilian Portuguese.
- **Highly Configurable**: All parameters are controlled via environment variables, making it perfect for CI/CD environments.

## 🚀 How It Works

1.  **Trigger**: The tool is designed to be run in a CI/CD pipeline when a merge request is created or updated.
2.  **Fetch Diffs**: It fetches the code changes (diffs) from the specified GitLab merge request.
3.  **Analyze**: It sends the code, along with contextual information from the MR title and description, to the Gemini API for analysis.
4.  **Post Feedback**: It parses the AI's response and posts feedback to GitLab:
    - A general summary note.
    - Discussions attached to specific lines of code for each identified issue.
5.  **Integrate with Jira**:
    - It extracts the Jira ticket key from the MR title (e.g., `PROJ-123`).
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

review_job:
  stage: test
  image: python:3.11-slim
  before_script:
    - pip install gitlab-gemini-reviewer # Or install from your private registry
  script:
    - gemini-reviewer
  variables:
    # It's highly recommended to store these as protected CI/CD variables in GitLab
    GITLAB_TOKEN: $GITLAB_TOKEN
    GEMINI_API_KEY: $GEMINI_API_KEY
    JIRA_URL: $JIRA_URL
    JIRA_USER: $JIRA_USER
    JIRA_TOKEN: $JIRA_TOKEN
  rules:
    - if: $CI_PIPELINE_SOURCE == 'merge_request_event'
```

## 📄 License

This project is licensed under the MIT License.