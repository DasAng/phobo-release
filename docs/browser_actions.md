# Browser automation actions

**Browser**

 - [Launch browser](#launch-browser)
 - [Close browser](#close-browser)

**Page**
 - [Navigate to url](#navigate-to-url)
 - [Take screenshot](#take-screenshot)
 - [Any request error](#any-request-error)
 - [Exact request error](#exact-request-error)
 - [Page wait](#page-wait)
 - [Page click](#page-click)
 - [Page type](#page-type)
 - [Page query](#page-query)
 - [Page count queried items](#page-count-queried-items)
 - [Page queried items contains any,all or exact](#page-queried-items-contains-not,any,all-or-exact)
 - [Page url like](#page-url-like)
 - [Page reload](#page-reload)
 - [Page select dropdown](#page-select-dropdown)
 - [Page select by text dropdown](#page-select-by-text-dropdown)
 - [Page element is visible or hidden](#page-element-is-visible-or-hidden)
 - [Page clear localstorage](#page-clear-localstorage)
 - [Page delete cookie](#page-delete-cookie)
 - [Page set value](#page-set-value)
 - [Page save content](#page-save-content)

---

 ## Launch browser

You can launch Chrome/Edge browser either headless or visible. You can also specify the size of the browser window.

`Regex`:

```shell
/^A new (headless)?\s*(chrome|edge) browser\s*(\d+,\d+)?$/i
```

`match signature`:

```shell
a new [headless] <chrome|edge> browser [<width>,<height>]
```

- `[headless]`: an optional value of *headless* which indicates a headless browser.
- `<chrome|edge>`: browser type can either be *chrome* or *edge*
- `[<width>,<height>]`: an optional value for the *width* and *height* of the browser window

The matching is case insensitive and must match the entire text from beginning to end.

Example of usage:

- Launch a visible chrome browser

   > `a new chrome browser`

- Launch a headless chrome browser

   > `a new headless chrome browser`

- Launch a visible chrome browser with specified width and height

   > `a new chrome browser 1200,768`

- Launch a visible edge browser with specified width and height

   > `a new edge browser 1200,768`

- Launch a headless chrome browser with specified width and height

   > `a new headless chrome browser 1200,768`

- Launch a headless edge browser with specified width and height

   > `a new headless edge browser 1200,768`

## Close browser

Browser will automatically be closed a the end of each scenario without you having to explicitly specifying it. But if you want to close a browser in any particular step you can use the following action:

`Regex`:

```shell
/close browser/i
```

`match signature`:

```shell
close browser
```

The matching is case insensitive and can appear anywhere within the sentence.

Example of usage:

- Valid expressions

    > `close browser`


## Navigate to url

Navigate to a specific url. You must specify the url to navigate to. You can specify an optional timeout value to wait for the page to load.
If the page failed to load within the timeout period then it will still count as a successfull action. The timeout value is used to ensure that the page gets enough time to finish it's requests and load it's content. If you don't specify a timeout value the default timeout value of 5000ms will be used.

All network requests will be captured within the timeout period and will be displayed in the HTML report.

This action will fail if you have not already launched a browser with the action `a new browser` see [Launch browser](#launch-browser)

`Regex`:

```shell
/Navigate to ([^\s]+)\s*(timeout=[0-9]*[1-9][0-9]*)?/i
```

`match signature`:

```shell
navigate to <url> [timeout=<value>]
```

- `<url>`: url to navigate to
- `[timeout=<value>]`: an optional timeout value in milliseconds to wait for the page to load

The matching is case insensitive and can appear anywhere within the sentence.

Example of usage:

- Navigate to a url using default timeout value of 5000ms

  > `navigate to https://google.com`

- Set timeout value to 10000ms

   > `navigate to https://google.com timeout=10000`

## Take screenshot

This action will take a screenshot of the current page and save it. You can optionally specify the full name of the image file to store to disk otherwise the image will be embedded as part of the HTML report.


`Regex`:

```shell
/Take screenshot\s*(\S*)?/i
```

`match signature`:

```shell
take screenshot [<file_path>]
```

- `<file_path>`: an optional file path to store the screenshot image

The matching is case insensitive and can appear anywhere within the sentence.

Example of usage:

- Take a screenshot without storing to image file to disk

   > `take screenshot`

- Take a screenshot and store it as the file myscreenshot.png

   > `take screenshot myscreenshot.png`

## Any request error

This action will check if there are any network request errors


`Regex`:

```shell
/any request error/i
```

`match signature`:

```shell
any request error
```

The matching is case insensitive and can appear anywhere within the sentence.

Example of usage:

- Check if there were any network request errors

   > `any request error`

## Exact request error

This action will check if there are the specified exact number of request errors


`Regex`:

```shell
/(\d+) request error/i
```

`match signature`:

```shell
@count request error
```

- `@count`: the number of request errors to expect

The matching is case insensitive and can appear anywhere within the sentence.

Example of usage:

- Check if there are exactly 1 network request error

   > `1 request error`

## Page wait

This action will wait a number of specified milliseconds on the current page. It can be used to wait for a slow loading page in order for the page to be fully loaded.


`Regex`:

```shell
/page wait (\d+)/i
```

`match signature`:

```shell
page wait <wait_time>
```

- `<wait_time>`: number of milliseconds to wait

The matching is case insensitive and can appear anywhere within the sentence.

Example of usage:

- Wait for 5000ms:

    > `page wait 5000`


## Page click

This action will click on any element specified on the page. It can be used for example to click on a button.
The element to click on is specified using XPATH expression. If the element is not found this action will fail.

This action expects a page has been loaded and exist otherwise it will fail.


`Regex`:

```shell
/page click (.+)/i
```

`match signature`:

```shell
page click <query>
```

- `<query>`: an XPATH query string

The matching is case insensitive and can appear anywhere within the sentence.

Example of usage:

- Click on a button with the specified XPATH expression

    > `page click //*button[@id='mybutton']`


## Page type

This action will type the specified text into any element on the page that can accept input. To find the element you will need to specify an XPATH expression.

The text to type needs to be specified in double quotes, but the double quotes will be removed before typing in the text.

This action expects a page has been loaded and exist otherwise it will fail.

`Regex`:

```shell
/page type "(.+)" (.+)/i
```

`match signature`:

```shell
page type "<text>" <query>
```

- `<text>`: the text value to enter. You can use an [Intrinsic expression](intrinsic_expression.md) for this parameter
- `<query>`: an XPATH query string

The matching is case insensitive and can appear anywhere within the sentence.

Example of usage:

- Type the word *hello world* into an input element

    > `page type "hello world" //*input[@id='myinput']`


## Page query

This action will find the specified element or elements using an XPATH expression.

The can be used if for example you want to find certain elements on the page. The items found will be saved in an internal list of queried items and can be used by other actions to check on them.

Items found will also be displayed in the HTML report.

This action expects a page has been loaded and exist otherwise it will fail.

`Regex`:

```shell
/page query (.+)/i
```

`match signature`:

```shell
page query <query>
```

- `<query>`: an XPATH query string

The matching is case insensitive and can appear anywhere within the sentence.

Example of usage:

- Find all elements on the page using the specified XPATH

    > `page query  /li/ul`


## Page count queried items

This action will count the number of elements last quried by any previous actions like for example `page query`.

The can be used for example to check if a number of elements are present.

This action will return success if the number of elements found are equal to the specified number. Otherwise this action will fail.

This action expects a page has been loaded and exist otherwise it will fail.

`Regex`:

```shell
/page count queried items are (\d+)/i
```

`match signature`:

```shell
page count queried items are <count>
```

- `<count>`: number of items to expect

The matching is case insensitive and can appear anywhere within the sentence.

Example of usage:

- Expect the last queried items is equal to 5

    > `page count queried items are 5`


## Page queried items contains not,any,all or exact

This action will check if the last queried items contains any, all or exact list of text strings. 

The can be used for example to check if a specific text string is present or if all specified text strings are present or if any of the specified text strings are present.

For example we have queried the elements of a table to get back the text elements:

- bob
- alice
- jack

Now we could use this action to check if bob exists in the queried items, or we could check if bob and alice is present or if bob, alice and jack are all present.

By using the keyword `not`, `any`, `all` or `exact` we can control how we want to match the text strings.

- `not`: must not match any of the specified text strings
- `any` will match any of the text strings we specify
- `all` must match all the text strings we specify
- `exact` must match exact only those text strings we specify

This action will return success if the queried items fullfills the matches required. Order of the text strings are not important.

This action expects a page has been loaded and exist otherwise it will fail.

`Regex`:

```shell
/page queried items contains (not|any|all|exact) text values\: (.+)/i
```

`match signature`:

```shell
page queried items contains <any|all|exact> text values: <text_list>
```

- `<not|any|all|exact>`: can either be one of *not*, *any*, *all* or *exact*
- `<text_list>`: a comma separated list of text values. fx *john,bob,alice*

The matching is case insensitive and can appear anywhere within the sentence.

Example of usage:

- Expect any one of bob, alice, jack to be present in the last queried elements

    > `page queried items contains any text values: bob,alice,jack`

- Expect all bob, alice, jack to be present in the last queried elements

    > `page queried items contains all text values: bob,alice,jack`

- Expect all last queried elements to have exactly and only the text strings bob, alice, jack in any order

    > `page queried items contains exact text values: bob,alice,jack`

- Expect none of the following text matches *bob,alice,jack*

    > `page queried items contains not text values: bob,alice,jack`


## Page url like

This action will check if the page url contains the specified url.

It can for example be used to ensure that we have loaded the correct url.

This action will return success if the page url contains the specified url, otherwise it will fail.

This action expects a page has been loaded and exist otherwise it will fail.

`Regex`:

```shell
/page url like ([^\s]+)/i
```

`match signature`:

```shell
page url like <value>
```

- `<value>`: a url or partial part of url to match

The matching is case insensitive and can appear anywhere within the sentence.

Example of usage:

- Expect the url to be https://google.com

    > `page url like https://google.com`

- Expect the url to contain the string ?query=10&name=bob

    > `page url like ?query=10&name=bob`


## Page reload

This action will reload the current page. A timeout value can be specified to wait for a period of time after the reload.

All network requests will be captured during the reload of the page and stored in the HTML report.

This action expects a page has been loaded and exist otherwise it will fail.

`Regex`:

```shell
/page reload\s*(timeout=[0-9]*[1-9][0-9]*)?/i
```

`match signature`:

```shell
page reload [timeout=<value>]
```

- `[timeout=<value>]`: an optional timeout value in milliseconds

The matching is case insensitive and can appear anywhere within the sentence.

Example of usage:

- Reload page

    > `page reload`

- Reload page and set timeout to 5000ms

    > `page reload timeout=5000`


## Page select dropdown

This action will select a value from a dropdown or in HTML a `<select>` element. You will need to specify a CSS query selector to find the HTML `<select>` element and then you will specify the value of the HTML `<option>` element to select.

So for example if you have a HTML like the following:

```html
<select id="language_select">
 <option value="1">English</option>
 <option value="2">Spanish</option>
 <option value="3">German</option>
</select>
```

you can use the action to select the option *Spanish* like so:

> `page select doprdown #language_select : 2

You can select multiple options if is is multi-selected `<select>` element using a comma separated list of values:

> `page select doprdown #language_select : 2,3

The items selected will be stored in the HTML report.

This action expects a page has been loaded and exist otherwise it will fail.

`Regex`:

```shell
/page select dropdown (.+) \: (.+)/i
```

`match signature`:

```shell
page select dropdown <query> : <value>
```

- `<query>`: a CSS query
- `<value>`: the value to select

The matching is case insensitive and can appear anywhere within the sentence.

Example of usage:

- Select the dropdown with the CSS query #select_language and the value of 1

    > `page select dropdown #select_language : 1`

- If select is a multi-select element then you can select multiple values using comma separated list:

    > `page select dropdown #select_language : 1,2`

## Page select by text dropdown

This action differs from the `page select dropdown` action by matching the text of the displayed option instead of selecting the value directly.

So imagine you have an HTML looking like this:

```html
<select id="language_select">
 <option value="1">English</option>
 <option value="2">Spanish</option>
 <option value="3">German</option>
</select>
```

So if you use this action like the following:

> `page select by text dropdown #language_select : Spanish`

It will attempt to find the option element where the displayed content is equal to *Spanish* and select the value of *2*.

You can select multiple options if is is multi-selected `<select>` element using a comma separated list of values:

> `page select by text dropdown #language_select : Spanish,German`

The items selected will be stored in the HTML report.

This action expects a page has been loaded and exist otherwise it will fail.

`Regex`:

```shell
/page select by text dropdown (.+) \: (.+)/i
```

`match signature`:

```shell
page select by text dropdown <query> : <value>
```

- `<query>`: a CSS query
- `<value>`: the value to select

The matching is case insensitive and can appear anywhere within the sentence.

Example of usage:

- Select the dropdown with the CSS query #select_language and the value Spanish

    > `page select dropdown #select_language : Spanish`

- If select is a multi-select element then you can select multiple values using comma separated list:

    > `page select dropdown #select_language : Spanish,German`


## Page element is visible or hidden

This action will check if the specified element(s) on the page is visible or hidden. Hidden can also mean that the element is not found on the page.

Use the keyword `visible` to check if the element(s) is visible and if it is the action will return success otherwise it will fail.

Use the keyword `hidden` to check if the element(s) is hidden or not found on the page. If the element(s) is found then the action will fail.

To find the element you will need to use an XPATH query.

This action expects a page has been loaded and exist otherwise it will fail.

`Regex`:

```shell
/page element is (visible|hidden)\s*\: (.+)/i
```

`match signature`:

```shell
page element is <visible|hidden>: <query>
```

- `<visible|hidden>`: can be either *visible* or *hidden*
- `<query>`: an XPATH query

The matching is case insensitive and can appear anywhere within the sentence.

Example of usage:

- Check if element(s) is visible using specified XPATH query

    > `page element is visible : //*div/li/a[1]`

- Check if element(s) is hidden using specified XPATH query

    > `page element is hidden : //*div/li/a[1]`

## Page clear localstorage

This action will clear the localstorage for the current page domain.

This action expects a page has been loaded and exist otherwise it will fail.

`Regex`:

```shell
/page clear localstorage/i
```

`match signature`:

```shell
page clear localstorage
```

The matching is case insensitive and can appear anywhere within the sentence.

Example of usage:

- Clear all localstorage for current page domain

    > `page clear localstorage`


## Page delete cookie

This action will delete the cookie specified in the name and for the specified domain.

This action expects a page has been loaded and exist otherwise it will fail.

`Regex`:

```shell
/page delete cookie: name=(.+)(?:domain=(.+))?/i
```

`match signature`:

```shell
page delete cookie: name=<name> [domain=<domain>]
```

- `<name>`: name of cookie
- `<domain>`: domain name

The matching is case insensitive and can appear anywhere within the sentence.

Example of usage:

- Delete a cookie named *mycookie*

    > `page delete cookie: name=mycookie`

- Delete a cookie named *mycookie* with domain *mydomain*

    > `page delete cookie: name=mycookie domain=mydomain`


## Page set value

This action will set the specified value of any element on the page. To find the element you will need to specify an XPATH expression.

The value to set needs to be specified in double quotes, but the double quotes will be removed before setting in the value.

This action expects a page has been loaded and exist otherwise it will fail.

`Regex`:

```shell
/page set value "(.+)" (.+)/i
```

`match signature`:

```shell
page set value "<text>" <query>
```

- `<text>`: the text value to set
- `<query>`: an XPATH query string

The matching is case insensitive and can appear anywhere within the sentence.

Example of usage:

- Set the value *hello world* into an input element

    > `page set value "hello world" //*input[@id='myinput']`

## Page save content

This action will save the HTML content of the page to a file on disk.

This action expects a page has been loaded and exist otherwise it will fail.

`Regex`:

```shell
/page save cotnent (.+)/i
```

`match signature`:

```shell
page save content <filePath>
```

- `<filePath>`: the file to save on disk. You can use an [Intrinsic expression](intrinsic_expression.md) for this parameter

The matching is case insensitive and can appear anywhere within the sentence.

Example of usage:

- Saves the HTML content of the page to a file named myfile.html

    > `page save content myfile.html`