# flashcard

VueJS components to add spaced-repetition based flashcard to any webpage.

## Demo: https://codepen.io/eshnil/pen/BaowoEW

Authors can create their article in a medium that helps their readers not just read, but also REMEMBER their content.
These components make it super-easy to achieve this without requiring any servers. LocalStorage is used to store user's progress on each question.

![image](https://user-images.githubusercontent.com/51651338/80756518-30143100-8b22-11ea-9fdc-2a0ff13ca007.png)

In your webpage, you would use the PracticeSet component specifying a bunch of question-answer pairs, and it will
use localStorage to store user's recall performance in order to determine practice frequency. 

See [FlashCard.vue](https://github.com/learn-awesome/flashcard/blob/master/components/FlashCard.vue) and [CardSet.vue](https://github.com/learn-awesome/flashcard/blob/master/components/CardSet.vue) to learn more about how this works.

This was built by the https://learnawesome.org community, inspired by https://quantum.country

# Develop

Install Vue CLI with: `npm install` and then serve:

`./node_modules/.bin/vue serve -o App.vue`

# Build

The umd files (which also includes CSS) in `dist` folder are generated with:
`./node_modules/.bin/vue-cli-service build --target lib --name CardSet components/CardSet.vue`

See `demo.html` for example of how to use the web-component version in plain-old HTML pages without any module bundlers
