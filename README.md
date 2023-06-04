# Make Repo Public Action

This action changes the visibility of a GitHub repository to public at a specified date and time.

## Inputs

### `target_date`

**Required** The date and time when the repository should be made public. This should be a Unix timestamp.

## Outputs

None.

## Example usage

This action requires a personal access token with the necessary permissions to change the repository's visibility. You should add this token as a secret in your repository's settings and name it `GH_PAT`.

You should also add the target date as a secret in your repository's settings and name it `TARGET_DATE`.

Here's an example of how to use this action:

```
yaml
name: Make Repo Public

on:
  schedule:
    - cron: '0 * * * *' # This will run the action every hour

env:
  VISIBILITY_CHANGED: 'false'

jobs:
  make-public:
    runs-on: ubuntu-latest
    steps:
      - name: Check date and change visibility
        id: change-visibility
        uses: your-github-username/make-repo-public-action@v1
        with:
          target_date: ${{ secrets.TARGET_DATE }}
      - name: Disable workflow
        if: env.VISIBILITY_CHANGED == 'true'
        run: |
          curl \
            -X PUT \
            -H "Authorization: token ${{ secrets.GH_PAT }}" \
            -H "Accept: application/vnd.github.v3+json" \
            https://api.github.com/repos/${{ github.repository }}/actions/workflows/make_repo_public.yml/disable
```

In this example, replace your-github-username with your actual GitHub username, and make-repo-public-action with the actual name of your repository. The @v1 at the end is the tag of the release that you want to use.

Please note that this action will permanently disable the workflow after it has successfully changed the repository visibility. If you want to run it again, you'll need to manually enable it in the Actions tab of your repository.
