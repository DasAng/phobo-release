# Plugin API reference

**Table of contents**

- [class BasePlugin](#class-baseplugin)
    - [match(pattern,fn)](#match(pattern,fn))
    - [execute()](#execute)
    - [run()](#run)
    - [attach(data,mediaType)](#attach(data,mediaType))
    - [member: page](#member:-page)
    - [member: stepResult](#member:-stepResult)
- [class ProxyPage](#class-proxypage)
    - [navigateTo(url,timeout)](#navigateto(url,timeout))
    - [takeScreenshot(filePath)](#takescreenshot(filepath))
    - [getNetworkRequests()](#getnetworkrequests())
    - [wait(timeout)](#wait(timeout))
    - [click(xPathQuery)](#click(xpathquery))
    - [type(xPathQuery,text)](#type(xpathquery,text))
    - [query(xPathQuery)](#query(xpathquery))
    - [getLastQueriedItemsAsText()](#getlastquerieditemsastext())
    - [url()](#url())
    - [reload(timeout)](#reload(timeout))
    - [selectDropdown(query,values)](#selectdropdown(query,values))
    - [selectByTextDropdown(query,values)](#selectbytextdropdown(query,values))
    - [isVisible(xPathQuery)](#isvisible(xpathquery))
    - [deleteCookie(name,domain)](#deletecookie(name,domain))
    - [clearLocalstorage()](#clearlocalstorage())


# Class BasePlugin

This is the base class for all plugins. A plugin must create a class that extends the *BasePlugin* class. The BasePlugin class wraps functionality to communicate between Phobo and plugins. It also instantiates an instance of [class ProxyPage](#class-proxypage) and exposes is as the member variable [page](#fields:-page). Plugins can then use the [page](#fields:-page) variable to interact with the browser.

Plugins must override the method [execute](#execute) to implement their own actions. Use the method [match](#match) to define match patterns and related actions.

Below is an example implementation of a plugin class that extends the BasePlugin class and overrides the execute method:

```js
import BasePlugin from "./proxies/BasePlugin";

class demo extends BasePlugin {
    
    protected execute(): Promise<unknown> {

        return this.match(/^demo plugin/i, async (match: any) => {
            console.log("Found match: ", match);
        })
        .then(() => {
            
            return this.match(/^demo navigate (.+)/i, async (match: any, url: string) => {
                console.log("Navigate to url: ", url);
                await this.page.navigateTo(url, 5000);
                console.log("done waiting for action");
            })
        })
    }
}

(async() => {
    await new demo().run();
})();
```

## Match(pattern,fn)

- `pattern`: \<Regexp> a regex pattern for matching
- `fn`: \<Function> a function to run if the pattern matches
- returns \<[BasePlugin](#class-baseplugin)>

Defines a match pattern which if matches will trigger the function **fn** to be run. You will usually call this function inside your [execute()](#excute) method.

The function **fn** takes a variable number of parameters:

```js
async function(match: string, ...)
```

The first parameter is always the entire match and subsequent parameters are present for each capturing group in your regex match pattern.

## execute()

- returns: \<Promise>

All derived classes from BasePlugin must override this method. You will implement your own code inside this method to match and execute actions. You will usually call the [match()](#match) functions one or more times in this method. Here is an example of how to override this method and use the match function to execute actions:

```js
protected execute(): Promise<unknown> {

    return this.match(/^I sign in to mywebsite.com/i, async (match: any) => {
        // ... add code to sign into mywebsite.com
        console.log("Found match: ", match);
    })
    .then(() => {
        
        return this.match(/^I sign out of (.+)/i, async (match: any, url: string) => {
            // add code to sign out of whatever url is captured in the pattern
        })
    })
}
```

The execute() method is also the one being called by the [run()](#run) method.

## run()

- returns: \<void>

This method must be called to actually start executing your plugin. It will internally call the method [execute()](#execute) which you must override in your own class.

Example instantiating an instance of your plugin class and call run():

```js
(async() => {
    await new demo().run();    
})();
```

## attach(data,mediaType)

- `data`: \<string> can be a JSON stringified data or fx a base64 encoded image or any text string value.
- `mediaType`: \<string> the type of the attachment fx. *application/json* or *text/plain* etc.
- returns: \<void>

This method will store the data as an attachment for the specified step. It will be embedded in the HTML migoi report and how it is displayed depends on the *mediaTyp* parameter. For example if mediaType is *application/json* then the HTML report will display the data in a nice JSON formatted view.

Example attaching a JSON value:

```js
attach(JSON.stringify({name: "hello"}), "application/json")
```

## member: page

- page: \<[ProxyPage](#class-proxypage)>

This member variable is an instance of the [ProxyPage](#class-proxypage) class. It exposes methods that you can use to interact with the Browser. For example it allows you to navigate to a specific url using the method [page.navigateTo()](#navigateto):

Here is an example of how you can use the page instance to navigate to a url:

```js
return this.match(/^I navigate to (.+)/i, async (match: any, url: string) => {
    await this.page.navigateTo(url, 5000);
})
```

## member: stepResult

- stepResult: \<any>

This member variable is can be used to set the result of the step executed by the plugin. This is used to set the result of running your plugin. For example if your plugin is doing a calculation of some sort and the result is a number you can store this result in the *stepResult* variable and Phobo will add it to the result of the step.

Here is an example of how to store your result:

```js
return this.match(/^I add 2+2 is /i, async (match: any) => {
    const result = 2+2;
    this.stepResult = result;
})
```

# Class ProxyPage

This class despite the name is a class that exposes methods to interact with the browser, such as navigating to url, take screenshot, click on button etc.

The class is a proxy class because it wraps around the messaging protocol that is used to send messages to Phobo to instruct Phobo how to interact with the browser.
The BasePlugin class will instantiates an object of this class and expose that object as the [page](#member:-page) member variable, so that you can use it from your derived plugin class.

## navigateTo(url,timeout)

- `url`: \<string> a url to navigate to
- `timeout`: \<number> a timeout value in milliseconds. The page will wait for this amount before timing out. This is used to ensure that a certain elapsed time has occurred so that the page has enough time to load its contents.
- returns: \<Promise>

This methid is used to navigate to a url and wait for the page to load within the given timeout value. All network requests will be saved internally and can be retrieved by calling the method [getNetworkRequest()](#getnetworkrequests).

## takeScreenshot(filePath)

- `filePath`: \<string|undefined> name of the image file to save to local disk. If ommitted then the image will not be saved to disk, but instead will appear as part of the HTML report generated.
- returns: \<Promise>

This method will take a screenshot of the current web page and optionally save it to disk.

## getNetworkRequests()

- returns: \<Promise<NetworkRequest[]>> a list of network request objects

This method will return a history of network request objects obtained from loading a web page. The network request object has the following definition:

```js
interface NetworkRequest {

    url: string | null | undefined;
    status: number;
    method: string | null | undefined;
    statusText: string | null | undefined;
    json: any | null | undefined;
}
```

- `url`: the url of the request
- `status`: the response status code, fx 200 or 400 etc.
- `method`:  the request method fx: GET, POST, OPTION
- `statusText`: the reponse message if any
- `json`: the response json data if any

## wait(timeout)

- `timeout`: \<number> a timeout value in milliseconds to wait for the page to laod. This is used to pause waiting for page to finish loading or doing network requests etc.
- returns: \<Promise>

This method can be used to wait for a specified amount of milliseconds so that a page has time to load or perform network requests.

## click(xPathQuery)

- `xPathQuery`: \<string> an XPATH query expression used to find the element on the page to click on.
- returns: \<Promise<string[]>> a list of items that has been clicked on as a stringified HTML

This method is used to find and click on any element on the page. Finding the element is done by using an XPATH query expression. The returned value is the innerhHTML of the items that was clicked on.

## type(xPathQuery,text)

- `xPathQuery`: \<string> an XPATH query expression used to find the element to enter text into.
- `text`: \<string> the text to enter
- returns: \<Promise<string[]>> a list of text values of the elements that was entered text on.

This method enters the specified text into the elements found by the XPATH query. This can be used for example to enter text into a text field. The return value is a list of the text values from the elements that was entered text on.

## query(xPathQuery)

- `xPathQuery`: \<string> an XPATH query expression used to find the element
- returns: \<Promise<string[]>> the inner HTML string of the elements found

This method is used to find elements on the page using an XPATH query.

## getLastQueriedItemsAsText()

- returns: \<Promise<string[]>> the text values of the last queried elements

This method is used to return the text values of the last queried elements.

## url()

- returns: \<Promise<string>> the url of the page

This method returns the url of the current page

## reload(timeout)

- `timeout`: \<numbner> a timeout value in milliseconds to wait after the page reloads
- returns: \<Promise>

This method will reload the current page. All network requests will be saved.

## selectDropdown(query,values)

- `query`: \<string> a CSS query expression used to find the HTML \<select> element
- `values`: \<string> a comma separated list of values to select. If the select is a multi-select element then each value will be selected.
- returns: \<Promise<string[]>> the value of the selected items

This method will use a CSS query expression to find the dropdown element to select values. It can be used to select mulitple values if the dropdown supports multi-select. This is done by specifying a comma separated list of values in the parameter `values`.

Example find the dropdown by id *#language* and select multiple option values 1,2,3:

```js
this.page.selectDropddown('#language', '1,2,3')
```

## selectByTextDropdown(query,values)

- `query`: \<string> a CSS query expression used to find the HTML \<select> element
- `values`: \<string> a comma separated list of option texts to select. If the select is a multi-select element then each option will be selected.
- returns: \<Promise<string[]>> the value of the selected items

This method is almost similar to the [selectDropdown(query,values)](#selectdropdown(query,values)) method but has a subtle difference. The `values` field is the display text instead of the HTML value.
It is better to illustrate this by using an example. So let's say we have the following select element:

```html
<select id="#language">
    <option value="1">English</option>
    <option value="2">German</option>
    <option value="3">French</option>
</select>
```

If we used the [selectDropdown](#selectdropdown(query,values)) method we would specify the `values` field as "1" or "2" or "3":

```js
this.page.selectDropddown('#language', '1')
```

And it will select the option with the value of 1, which would be English.

Now instead if we wanted to select by the display value of "English" instead we could use this method like so:

```js
this.page.selectByTextDropdown('#language', 'English')
```

And it will select the option with value of 1, because it will match the option that has the text set to "English".

## isVisible(xPathQuery)

- `xPathQuery`: \<string> an XPATH query expression to check if an element is visible on the page
- returns: \<Promise<boolean>> true if the item is visible otherwise false

This method will check if an element on the page is visible or not.

## deleteCookie(name,domain)

- `name`: \<string> name of cookie
- `domain`: \<string> optional domain name
- returns: \<Promise>

This method delete the cookie with the specified name and optional with the specified domain.

## clearLocalstorage()

- returns: \<Promise>

This method will clear all localstorage items for the current domain