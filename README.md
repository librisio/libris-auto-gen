# Libris Auto-Generate GitHub Action
<p>
    <img src="https://img.shields.io/badge/version-1.0.0-blue" alt="Libris">
    <img src="https://img.shields.io/badge/status-maintained-forestgreen" alt="Libris">
</p> 

**Boost productivity with Libris: Automatically generate comprehensive modern documentation for your projects.**

![Libris Docs Banner](https://raw.githubusercontent.com/librisio/.github/master/media/github/readme_banner_rounded.png)

# Libris
Seamlessly integrate Libris with your development workflow to guarantee your documentation remains up-to-date. Our action automates the generation of documentation each time you push to your repository, ensuring accuracy and consistency without manual effort.

* [x] Automatic Documentation Updates: Eliminates the need for manual updates, ensuring your documentation always matches your latest codebase.
* [x] Seamless Integration: Easily integrates with your existing GitHub workflow, making it a hassle-free addition to your development process.
* [x] Consistency and Accuracy: Maintains high standards of consistency and accuracy in documentation with every code change.
* [x] Time-Saving: Frees up valuable developer time by automating repetitive tasks, allowing your team to focus on development and innovation.

# Integrating the Action into Your Repository

To facilitate automatic documentation updates within your repository, the workflow action requires write permissions and access to the GitHub-generated `GITHUB_TOKEN` secret. This guide outlines the steps to seamlessly integrate this action into your workflow.

## Setting Up the GitHub Workflow File

Begin by creating a GitHub workflow action file named `libris.yaml` within the `.github/workflows` directory of your repository. This action automates the documentation generation process, ensuring your project's documentation remains up-to-date with every push.

### Configuration Inputs:

* `config`: Specify the relative path to your Libris configuration file, which can be either a JSON or JavaScript file.
* `output`: Define the relative path for the output of the generated documentation.
            **Important**: The action will overwrite existing data at this location, so ensure the output path is correctly specified to avoid unintended data loss.

###### .github/workflows/libris.yaml

```yaml
# Libris Auto-Generate Documenation Action.
#   Seamlessly integrate Libris with your development workflow to guarantee your documentation remains up-to-date.
#   Our action automates the generation of documentation each time you push to your repository, ensuring accuracy and consistency without manual effort.

name: Libris - Generate Documentation

# Grant write permissions in order to write the generated documentation file to your repo.
permissions:
  contents: write

# Trigger on every push.
on: push

jobs:
  update-file:
    runs-on: ubuntu-latest
    steps:

      # Checkout.
      - name: Checkout code
        uses: actions/checkout@v4

      # Optionally install Node.js and dependencies.
      # - name: Setup Node.js
      #   uses: actions/setup-node@v4
      #   with:
      #     node-version: '20'
      # - name: Install Dependencies
      #   run: npm ci
      #   # run: npm install @librisio/libris
      
      # Generate documentation.
      - name: Libris - Generate Documentation
        uses: librisio/libris-action@main
        env:

          # Your secret github token.
          # This token is automatically generated by github.
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }} # 

          # Your Libris API Key.
          # Must manually be created under "Settings > Secrets and variables > Actions > New repository secret".
          LIBRIS_API_KEY: ${{ secrets.LIBRIS_API_KEY }}

        with:

          # The relative input path of your configuration file, may either be a JSON or JavaScript file.
          config: './docs/config.js'

          # The relative output path for the generated documentation file.
          output: './docs/index.html'

          # The branch where the new edits will be pushed to. Leave "" to use the branch that is being pushed.
          branch: 'docs'
```

## Adding Your Libris API Key to Repository Secrets

For the action to access the Libris API, you must provide your Libris API Key as a secret in your repository.

1. In your repository navigate to `Settings` > `Secrets and variables` > `Actions`.
2. Click `New repository secret` to add a new secret.
3. Name the secret `LIBRIS_API_KEY` and paste your Libris API Key into the value field.

For more details on obtaining your Libris API Key, consult the [Libris documentation](https://uselibris.io/docs?id=Authentication:API%20Key).

<!--
## Optional: Exclude Output from Version Control

To prevent unnecessary pull requests triggered by automated documentation updates, consider adding the output HTML file to your `.gitignore`. This step ensures that the generated documentation does not clutter your repository's version history.

###### .gitignore
```
docs/index.html
```
-->