# launcher-products-ms-nest

- Launcher Products Microservice

## Developments

1. Clone the repository
2. Create a .env file
3. Run `git submodule update --init --recursive` to update submodule
4. Run `docker compose up --build` to start the project

## Using git submodule

- Add the submodule, where repository_url is the repository URL and directory_name is the name of the folder where you want to save the submodule (it should not already exist in the project)

`git submodule add <repository_url> <directory_name>`

- To eliminate the submodule, use the command 

`git submodule deinit <directory_name>`

- To eliminate the submodule directory de git, use the command
  
`git rm -f <directory_name>`

- to eliminate the submodule folder, use the command

`rm -rf .git/modules/<directory_name>`

- to update the submodule, use the command

`git submodule update --init --recursive`