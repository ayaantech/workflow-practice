# workflow-practice

this is a repository with a few examples of basic workflows

```yaml
# This is a basic workflow to help you get started with Actions

name: Simple workflow

# Controls when the workflow will run
on:
  # Triggers the workflow on push or pull request events but only for the "main" branch
  #push:
  #  branches: [ "main" ]
  #pull_request:
  #  branches: [ "main" ]

  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:

# A workflow run is made up of one or more jobs that can run sequentially or in parallel
jobs:
  # This workflow contains a single job called "build"
  first_sequential_job:
    name: First job running by itself
    # The type of runner that the job will run on
    runs-on: windows-latest

    # Steps represent a sequence of tasks that will be executed as part of the job
    steps:
      # Checks-out your repository under $GITHUB_WORKSPACE, so your job can access it
      - uses: actions/checkout@v3
        name: Checkout the workflow-practice repository
      # Runs a single command using the runners shell
      - name: Show checked out files, create a new file
        run: |
          echo "Showing the repository as-is"
          ls
          echo "THIS FILE IS CREATED DURING THE WORKFLOW AND DOES NOT EXIST IN THE REPOSITORY" > readme.txt
          echo "Showing the repository after creating the files"
          ls
        shell: powershell
      - name: Read a file from the workflow-practice repository that we just checked out, and create a temporary folder
        run: |
          echo "Reading a file"
          cat ${{ github.workspace }}\readme.txt
          echo "Creating a folder"
          mkdir ${{ github.workspace}}\created_folder
          echo "Showing the folder that we created"
          ls ${{ github.workspace }}
        shell: powershell
      - name: Move the readme file
        run: |
          echo "Before move"
          ls
          mv ${{ github.workspace }}\readme.txt ${{ github.workspace }}\created_folder
          echo "After move"
          ls
          echo "Showing the contents of ${{ github.workspace }}\created_folder"
          ls ${{ github.workspace }}\created_folder
        shell: powershell
  second_parallel_job_1:
    # The type of runner that the job will run on
    runs-on: windows-latest
    name: The first job that runs in parallel with the other one
    needs: first_sequential_job
    # Steps represent a sequence of tasks that will be executed as part of the job
    steps:
      # Checks-out your repository under $GITHUB_WORKSPACE, so your job can access it
      - uses: actions/checkout@v3
        name: Checkout the workflow-practice repository
      - name: Show checked out files
        run: |
          echo "Are my files still there?"
          ls
          echo "No, they are not. this is a completely new runner / system!"
        shell: powershell
  second_parallel_job_2:
      # The type of runner that the job will run on
      runs-on: windows-latest
      name: The second job that runs in parallel with the other one
      needs: first_sequential_job
      # Steps represent a sequence of tasks that will be executed as part of the job
      steps:
        # Checks-out your repository under $GITHUB_WORKSPACE, so your job can access it
        - uses: actions/checkout@v3
          name: Checkout the workflow-practice repository
        - name: Show checked out files
          run: |
            echo "Are my files still there?"
            ls
            echo "No, they are not. this is a completely new runner / system!"
          shell: powershell
```
