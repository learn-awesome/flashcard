# flashcard

VueJS components to add spaced-repetition based flashcard to any webpage. Less than 5KB gzipped, and 15KB unzipped.

Authors can create their article in a medium that helps their readers not just read, but also REMEMBER their content.
These components make it super-easy to achieve this without requiring any servers. LocalStorage is used to store user's progress on each question.

![image](https://user-images.githubusercontent.com/51651338/80756518-30143100-8b22-11ea-9fdc-2a0ff13ca007.png)

In your webpage, you would use the `CardSet` component specifying a bunch of question-answer pairs, and it will
use localStorage to store user's recall performance in order to determine practice frequency. 

See [FlashCard.vue](https://github.com/learn-awesome/flashcard/blob/master/components/FlashCard.vue) and [CardSet.vue](https://github.com/learn-awesome/flashcard/blob/master/components/CardSet.vue) to learn more about how this works.

This was inspired by https://ncase.me/remember/ and https://quantum.country

# Build

The UMD files (which also includes CSS) in `dist` folder are generated with:

Build: `./node_modules/.bin/vue-cli-service build --target lib --name CardSet components/CardSet.vue`
Test: [demo.html](demo.html)

# TODO: The Web component build is currently not working correctly.
Build: `./node_modules/.bin/vue-cli-service build --target wc --name fc 'components/*.vue'`
Test: [webcomp.html](webcomp.html)
