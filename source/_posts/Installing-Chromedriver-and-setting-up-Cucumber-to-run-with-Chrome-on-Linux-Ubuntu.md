title: Installing Chromedriver and setting up Cucumber to run with Chrome on Linux/Ubuntu
date: 2014-10-11 19:14:32
tags: Ubuntu Chromedriver Cucumber
---

We use [Cucumber](http://cukes.info/) to test the Behaviour of our application at [SupportBee](https://supportbee.com). While the tests run superbly on our [CI](https://circleci.com/), it is a bit of a problem to run the tests locally. Cucumber test's the behaviour of your application by running it in a browser, Firefox by default. While running the tests we get the following error:

```
unable to obtain stable firefox connection in 60 seconds (127.0.0.1:7055) 
(Selenium::WebDriver::Error::WebDriverError)
```
The problem has been discussed on [Stackoverflow](http://stackoverflow.com/questions/14303161/unable-to-obtain-stable-firefox-connection-in-60-seconds-127-0-0-17055), but the answer did not work for me either. So instinctively I decided to use Chrome instead of Firefox, and [bazinga](http://www.urbandictionary.com/define.php?term=Bazinga), it worked!

You can follow the below steps to install chrome webdrivers on Ubuntu (14.04.1 in my case)

- Install Unzip

`sudo apt-get install unzip`

- Assuming you're running a 64-bit OS, download the latest version of chromedriver from their [website](http://chromedriver.storage.googleapis.com/index.html)

```
wget -N http://chromedriver.storage.googleapis.com/2.10/chromedriver_linux64.zip -P ~/Downloads
```

```
unzip ~/Downloads/chromedriver_linux64.zip -d ~/Downloads
```

- Make the file you downloaded executable, and move it to `/usr/local/share`

```
chmod +x ~/Downloads/chromedriver
sudo mv -f ~/Downloads/chromedriver /usr/local/share/chromedriver
```

- Also, create symlinks to the chromedriver. Cucumber would look for the drivers in `/usr/bin/chromedriver`
```
sudo ln -s /usr/local/share/chromedriver /usr/local/bin/chromedriver
sudo ln -s /usr/local/share/chromedriver /usr/bin/chromedriver
```

- Finally, add/replace the below code in your `features/support/env.rb`

```
Capybara.register_driver :chrome do |app|
  # optional
  client = Selenium::WebDriver::Remote::Http::Default.new
  # optional
  client.timeout = 120
  Capybara::Selenium::Driver.new(app, :browser => :chrome, :http_client => client)
end
```

`Capybara.javascript_driver = :chrome`


And, you're good to go! Happy testing. \,,/
