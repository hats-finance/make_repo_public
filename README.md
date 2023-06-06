# Make Repo Public GitHub Action

This action changes the visibility of a GitHub repository to public at a specified date and time. It also calls a webhook with information about the repository, the target date, and whether the visibility change was successful.

## Inputs

- `target_date`: The date and time when the repository should be made public. This should be a Unix timestamp (the number of seconds since 1970-01-01 00:00:00 UTC). Required.
- `gh_pat`: A GitHub Personal Access Token with the `repo` scope. This is used to change the repository visibility and disable the workflow. Required.
- `webhook_url`: The URL of the webhook to call after the visibility change. Required.
- `verification`: A verification token to include in the webhook call. Required.

## Usage

First, you need to generate a GitHub Personal Access Token:

1. Go to your GitHub settings.
2. Click on "Developer settings".
3. Click on "Personal access tokens".
4. Click "Fine-grained tokens"
5. Click on "Generate new token".
6. Give your token a descriptive name.
7. Set the expiration date that further in the future then the date you want to make the repo public. This is the date when the token will no longer be valid.
8. Select the `Only select repositories` option.
9. Choose the relevant repo you would like to make public.
10. Choose `Repository permissions`
11. Allow `Actions` Read and write permissions.
12. Allow `Administration` Read and write permissions.
14. Click on "Generate token".
15. Copy the generated token.

Then, you need to add this token as a secret in your repository:

1. Go to your repository settings.
2. Click on "Secrets".
3. Click on "New repository secret".
4. Enter `GH_PAT` as the name and paste your token as the value.
5. Click on "Add secret".

Now, you can use the action in your workflow:

```yaml
- uses: your-username/make-repo-public@v1
  with:
    target_date: '[Unix time of when you want the repo to become public]'
    gh_pat: ${{ secrets.GH_PAT }}
    webhook_url: 'http://backend.hats.finance/test'
    verification: 'jggdfjg73r78gduigduy'
```

In this example, `your-username` should be replaced with your GitHub username, and `v1` should be replaced with the version of the action you want to use. The `target_date`, `gh_pat`, `webhook_url`, and `verification` inputs are set to example values and should be replaced with your own values.

Please note that this action should be used in a workflow that runs on a schedule and has the `make_repo_public.yml` workflow file. If the visibility change is successful, the workflow will be disabled to prevent further runs.

---

Please note that this README assumes that the action is published in the GitHub Marketplace under the name `make-repo-public` and that the user's GitHub username is `your-username`. You should replace these with your actual action name and username.
