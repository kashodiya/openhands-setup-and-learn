# OpenHands User Guide

## How to ask OpenHands to work on a story on an existing GitHub project
- Open OpenHands app in browser
- Click plus sign icon on top left
- Select your GitHub repo using dropdown under "Connect to a Repository"
- Click Launch button
- Watch bottom left message: Starting runtime...
- Wait till message becomes: Agent is awaiting user input...
- Type in the use story in the chat message box.
- Click VSCode tab. You will see your code. If you do not see, click Open In New Tab button.

## Advance concepts
### Run commands when your runtime container is created
- Create .openhands folder in your project
- Create .openhands/setup.sh
- OpenHands will run setup.sh everytime the runtime container is created
- For more complex needs consider creating your own runtime image


### System prompt
- Create .openhands/microagents folder in your project
- Create .openhands/microagents/repo.md
- AI Agent will read repo.md before doing any work




