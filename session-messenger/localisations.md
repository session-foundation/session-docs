# Localisations

Localisation will be taking place using Crowdin, a community-driven translation platform. You’ll need to [register a free Crowdin](https://accounts.crowdin.com/register) account to participate.

After you’ve registered an account, you can [find the Crowdin page for Session here](https://crowdin.com/project/session-crossplatform-strings).

From the dashboard you can search for the language you wish to help translate:

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

_**Note:** Because the starting point is a machine translation, Crowdin will display 100% or similar completion for all languages. Many of the strings **will still require review and translation, regardless of whether it is indicated that the translation is complete.**_

Once you’ve located the language you wish to help translate, click on it, then click on the ellipses (…) menu on the right side of the page:

<figure><img src="../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

Click on the ‘Editor View’ button at the top right of your screen, then click ‘Comfortable’

<figure><img src="../.gitbook/assets/image (2) (3).png" alt=""><figcaption></figcaption></figure>

On this page you will see the list of all strings and phrases used in Session.&#x20;

At this stage, all of Session's strings are approved, but there is still important work to be done! You can make a big difference to the quality of a localisation by reviewing it for errors.

In the middle of the screen, you’ll see the Source String at the top. This is the English phrase that is being used in the app.

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

#### \<b> \<br/> \<span>

These are used for formatting, making something bold, or adding a line break etc. When you’re translating, just read the sentence as if they’re not there, but please include them in or around the relevant sections of the phrase. In the example above the section mentioning “You” in your translated string should open with \<b> and close with\</b>. If you see \<br/>\<br/>, please try to include that at the same point in the phrase.

#### Curly Brackets { }

Anything in curly brackets should not be translated, as these are dynamic strings that will automatically adopt the right contents in the app. In the example above, {count} becomes the amount of other users who were invited to join the group.

#### Context

The Context section provides additional information about how the phrase is being used in the app. In this case, the phrase “You and {count} others were invited to join the group” shows as a control message in group conversations, which displays a user who was invited to join the group at the same time as several others.

#### Translating

Under the Context section, you’ll see a text box with “Enter translation here” and this is where you’ll write the translation in your chosen language for the English phrase “You and {count} others were invited to join the group.”

Once you’ve added that, just hit the save button, and select the next string from the list on the left.
