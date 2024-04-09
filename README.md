ANONYMOUS AMIT:
import os
import subprocess
from telegram import Update
from telegram.ext import Updater, CommandHandler, CallbackContext

# Define your Telegram bot token
TOKEN = "TELEGRAM_BOT_TOKEN"
# Function to handle the /start command
def start(update: Update, context: CallbackContext) -> None:
    update.message.reply_text("Welcome! Send /help to see available commands.")

# Function to handle the /help command
def help_command(update: Update, context: CallbackContext) -> None:
    help_text = """
    Available commands:
    /ls - List files in the current directory
    /pwd - Show current directory path
    /cd - Change directory
    /cat - Display file content
    /mkdir - Create a directory
    /git_clone - Clone a git repository
    /run_python - Run a Python script
    /run_pip - Install Python packages via pip
    /shell - Run shell commands
    """
    update.message.reply_text("
# Function to handle the /ls command
def ls_command(update: Update, context: CallbackContext) -> None:
    files = os.listdir('.')
    update.message.reply_text("
Files in current directory:\n" + '\n'.join(files) + "
# Function to handle the /pwd command
def pwd_command(update: Update, context: CallbackContext) -> None:
    cwd = os.getcwd()
    update.message.reply_text("
Current directory: " + cwd + "
# Function to handle the /cd command
def cd_command(update: Update, context: CallbackContext) -> None:
    args = context.args
    if len(args) == 1:
        directory = args[0]
        try:
            os.chdir(directory)
            update.message.reply_text("
Changed directory to: " + directory + "
        except FileNotFoundError:
            update.message.reply_text("
Directory not found: " + directory + "
    else:
        update.message.reply_text("Usage: /cd [directory]")

# Function to handle the /cat command
def cat_command(update: Update, context: CallbackContext) -> None:
    args = context.args
    if len(args) == 1:
        file_path = args[0]
        try:
            with open(file_path, 'r') as file:
                content = file.read()
                update.message.reply_text("
Content of " + file_path + ":\n" + content + "
        except FileNotFoundError:
            update.message.reply_text("
File not found: " + file_path + "
    else:
        update.message.reply_text("Usage: /cat [file_path]")

# Function to handle the /mkdir command
def mkdir_command(update: Update, context: CallbackContext) -> None:
    args = context.args
    if len(args) == 1:
        directory = args[0]
        try:
            os.mkdir(directory)
            update.message.reply_text("
Directory created: " + directory + "
        except FileExistsError:
            update.message.reply_text("
Directory already exists: " + directory + "
    else:
        update.message.reply_text("Usage: /mkdir [directory_name]")

# Function to handle the /git_clone command
def git_clone_command(update: Update, context: CallbackContext) -> None:
    args = context.args
    if len(args) == 1:
        repo_url = args[0]
        try:
            subprocess.run(["git", "clone", repo_url], check=True)
            update.message.reply_text("
Cloned repository successfully: " + repo_url + "
        except subprocess.CalledProcessError:
            update.message.reply_text("
Failed to clone repository: " + repo_url + "`", parse_mode='MarkdownV2')
    else:
        update.message.reply_text("Usage: /git_clone [repository_url]")

# Function to handle the /run_python command

def run_python_command(update: Update, context: CallbackContext) -> None:
    args = context.args
    if len(args) == 1:
        script_path = args[0]
        try:
            subprocess.run(["python", script_path], check=True)
        except subprocess.CalledProcessError:
            update.message.reply_text("
    else:
        update.message.reply_text("Usage: /run_python [script_path]")

# Function to handle the /run_pip command
def run_pip_command(update: Update, context: CallbackContext) -> None:
    args = context.args
    if len(args) >= 1:
        packages = ' '.join(args)
        try:
            subprocess.run(["pip", "install", packages], check=True)
            update.message.reply_text("
Successfully installed packages: " + packages + "```", parse_mode='MarkdownV2')
        except subprocess.CalledProcessError:
            update.message.reply_text("
    else:
        update.message.reply_text("Usage: /run_pip [package1] [package2] ...")

# Function to handle the /shell command
def shell_command(update: Update, context: CallbackContext) -> None:
    if not context.args:
        update.message.reply_text("Usage: /shell [command]")
        return
    command = ' '.join(context.args)
    try:
        result = subprocess.run(command, shell=True, capture_output=True, text=True)
        if result.returncode == 0:
            update.message.reply_text("
" + result.stdout + "```", parse_mode='MarkdownV2')
        else:
            update.message.reply_text("
    except Exception as e:
        update.message.reply_text("
An error occurred: " + str(e) + "```", parse_mode='MarkdownV2')

def main() -> None:
    # Initialize the Updater and pass it your bot's token.
    updater = Updater(TOKEN)

    # Get the dispatcher to register handlers
    dispatcher = updater.dispatcher

    # Register command handlers
    dispatcher.add_handler(CommandHandler("start", start))
    dispatcher.add_handler(CommandHandler("help", help_command))
    dispatcher.add_handler(CommandHandler("ls", ls_command))
    dispatcher.add_handler(CommandHandler("pwd", pwd_command))
    dispatcher.add_handler(CommandHandler("cd", cd_command))
    dispatcher.add_handler(CommandHandler("cat", cat_command))
    dispatcher.add_handler(CommandHandler("mkdir", mkdir_command))
    dispatcher.add_handler(CommandHandler("git_clone", git_clone_command))
    dispatcher.add_handler(CommandHandler("run_python", run_python_command))
    dispatcher.add_handler(CommandHandler("run_pip", run_pip_command))
    dispatcher.add_handler(CommandHandler("shell", shell_command))

    # Start the Bot
    updater.start_polling()

    # Run the bot until you press Ctrl-C or the process receives SIGINT, SIGTERM or SIGABRT
    updater.idle()

if name == 'main':
    main()
