# Libris Auto-Generate GitHub Action
<p>
    <img src="https://img.shields.io/badge/version-1.0.0-blue" alt="Libris">
    <img src="https://img.shields.io/badge/status-maintained-forestgreen" alt="Libris">
</p> 

**Boost productivity with Libris: Automatically generate comprehensive modern documentation for your projects.**

![Libris Docs Banner](https://raw.githubusercontent.com/librisio/.github/master/media/github/readme_banner_rounded.png)

## Libris
Seamlessly integrate Libris with your development workflow to guarantee your documentation remains up-to-date. Our action automates the generation of documentation each time you push to your repository, ensuring accuracy and consistency without manual effort.

* [x] Automatic Documentation Updates: Eliminates the need for manual updates, ensuring your documentation always matches your latest codebase.
* [x] Seamless Integration: Easily integrates with your existing GitHub workflow, making it a hassle-free addition to your development process.
* [x] Consistency and Accuracy: Maintains high standards of consistency and accuracy in documentation with every code change.
* [x] Time-Saving: Frees up valuable developer time by automating repetitive tasks, allowing your team to focus on development and innovation.

## Integrating the action into your repository
In order to write the generated documentation, the workflow action will require write permission and access to the by github generated secret `GITHUB_TOKEN`.

### Create the workflow file.
Create the workflow action file under `.github/workflows/libris.yaml`.

Optionally define different values for the inputs `config` and `output` if your file's are located under a different path.

**Warning**: Make sure the specified output path is properly defined. This workflow action will overwrite the file data at the specified output path.

###### .github/workflows/libris.yaml
```yaml
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
```

### Add the LIBRIS_API_KEY to your repository secrets.
Your Libris API Key must be defined in your repository secrets in order to access the Libris API.

To create a secret go to "Your repository -> Settings -> Secrets and variables -> Actions". Once there click the "New repository secret" to create a new secret. Assign the name `LIBRIS_API_KEY` to the secret and paste your Libris API Key into the value section.

More information about obtaining a Libris API Key can be found in the [documentation](https://uselibris.io/docs?id=Authentication:API%20Key).

### Add output to .gitignore (optional).
Since the workflow action writes the generated HTML file to your repository. It is advised to add the specified HTML output path to your `.gitignore`. Therefore, you will avoid the required pull requests after the workflow action has finished.

###### .gitinore
```
docs/index.html
```