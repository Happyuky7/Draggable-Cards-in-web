# Draggable-Cards-in-web

Draggable-Cards-in-web is a simple Script Draggable Cards + Website for example using and implementing.


## License

- [View License](https://github.com/HappyRogelio7/Draggable-Cards-in-web/tree/master/LICENSE)

## Using the Draggable Cards 

Using in very easy configurations

```js
// Script:
document.addEventListener('DOMContentLoaded', async () => {

    // Start Configuration
    // Configure the script

    // If is true then the script is in debug mode.
    let debug = false;
    // If is true then button clear storage is enabled in web.
    let clearbtnbolean = true;
    // value is the Clear LocalStorage button,
    // Required: clearbtnboolean is set to true and configuration
    //           button clear in html code.
    const valueClearBtn = 'clear-storage-btn'
    
    // The following values MUST be set for this script to work on your web site.

    // valueCardContainer is the class of the container of the cards.
    const valueCardContainer = '.card-container'

    // valueCard is the class of the cards.
    const valueCard = '.card'

    // IMPORTANT:
    // valueTargetCard is the class of the cards.
    // valueTargetCard is the same as valueCard, 
    // the only difference is that valueTargetCard does not have "." and valueCard does.
    // **IMPORTANT** without this the Script **DOES NOT WORK**.
    const valueTargetCard = 'card'

    // valueDataOrder is the data attribute of the cards that stores the order.
    // Value solo used Internal in this script OR LocalStorage variable.
    const valueDataOrder = 'data-order'

    // valueDataID is the data attribute of the cards that stores the id.
    const valueDataID = 'data-id'

    // valueCardOrderMap is the LocalStorage variable that stores the order of 
    // the cards.
    const valueCardOrderMap = 'cardOrderMap'

    // End Required values MUST be set for this script to work.

    // If is true then the script is in debug mode.
    if (debug === true) {
        console.log('Debug mode enabled');

        if (clearbtnbolean === true) {
            console.log('Clear LocalStorage button enabled');
            console.log('valueClearBtn: ' + valueClearBtn);
        } 
        
        console.log('Clear LocalStorage button disabled');
        
        console.log('valueCardContainer: ' + valueCardContainer);
        console.log('valueCard: ' + valueCard);
        console.log('valueDataOrder: ' + valueDataOrder);
        console.log('valueDataID: ' + valueDataID);
        console.log('valueCardOrderMap: ' + valueCardOrderMap);
    }

    // End Configuration


    // Start Script Code

    const cardContainer = document.querySelector(valueCardContainer);
    let draggedCard = null;

    // Function for storing the positions and contents of cards in the LocalStorage
    function saveCardData() {
        const cards = document.querySelectorAll(valueCard);
        cards.forEach((card, index) => {
            card.setAttribute(valueDataOrder, index + 1);
        });

        const cardOrderMap = {};
        cards.forEach((card) => {
            const cardId = card.getAttribute(valueDataID);
            const cardOrder = card.getAttribute(valueDataOrder);
            cardOrderMap[cardId] = parseInt(cardOrder);
        });

        localStorage.setItem(valueCardOrderMap, JSON.stringify(cardOrderMap));
        console.log('[SAVE] Card order saved in the LocalStorage.');
    }


    // Function for loading positions and stored content and positioning cards
    async function loadCardData() {
        return new Promise((resolve) => {
            const cardOrderMap = JSON.parse(localStorage.getItem(valueCardOrderMap));
            if (cardOrderMap) {
                const cards = document.querySelectorAll(valueCard);
                const sortedCards = [...cards].sort((a, b) => {
                    const cardIdA = a.getAttribute(valueDataID);
                    const cardIdB = b.getAttribute(valueDataID);
                    return cardOrderMap[cardIdA] - cardOrderMap[cardIdB];
                });

                // ADD card in correct order.
                sortedCards.forEach((card, index) => {
                    cardContainer.appendChild(card);
                    const cardId = card.getAttribute(valueDataID);
                    cardOrderMap[cardId] = index + 1;
                });

                // Delete the card, not exist in the DOM.
                const existingCardIds = Array.from(cards).map((card) => card.getAttribute(valueDataID));
                const storedCardIds = Object.keys(cardOrderMap);
                const removedCardIds = storedCardIds.filter((id) => !existingCardIds.includes(id));
                removedCardIds.forEach((removedId) => {
                    delete cardOrderMap[removedId];
                });

                // Save for changes in LocalStorage.
                localStorage.setItem(valueCardOrderMap, JSON.stringify(cardOrderMap));

                console.log('New card order:');
                sortedCards.forEach((card, index) => {
                    console.log(`${index + 1} = ${card.getAttribute(valueDataID)}`);
                });
            }
            console.log('[LOAD] Card order loaded from LocalStorage.');
            resolve();
        });
    }

    // Events of the Cards Order:

    // Save card position.
    cardContainer.addEventListener('dragend', () => {
        saveCardData();
    });

    // Start move card position.
    cardContainer.addEventListener('dragstart', (e) => {
        draggedCard = e.target;
    });

    // Check move area card position and save new position.
    cardContainer.addEventListener('dragover', (e) => {
        e.preventDefault();
        const targetCard = e.target;

        if (targetCard !== draggedCard && targetCard.classList.contains(valueTargetCard)) {
            const targetRect = targetCard.getBoundingClientRect();
            const draggedRect = draggedCard.getBoundingClientRect();

            if (e.clientY - targetRect.top < targetRect.height / 2) {
                cardContainer.insertBefore(draggedCard, targetCard);
            } else {
                cardContainer.insertBefore(draggedCard, targetCard.nextSibling);
            }

            console.log('New card order:');
            const cards = document.querySelectorAll(valueCard);

            cards.forEach((card, index) => {
                console.log(`${index + 1} = ${card.getAttribute(valueDataID)}`);
            });

            saveCardData(); 
        }
    });

    // If is true then button clear storage is enabled in web.
    // REQUIRES
    // 1. Add in HTML the button with id="clear-storage-btn" OR 
    //    your id set in valueClearBtn variable. 
    if (clearbtnbolean === true) {
        // Clear LocalStorage
        const clearStorageBtn = document.getElementById(valueClearBtn);
    clearStorageBtn.addEventListener('click', () => {
        localStorage.clear(); 
        loadCardData().then(() => {
            console.log('LocalStorage deleted and card positions reset.');
            window.location.reload();
        });
    });
    }


    // Load card position.
    await loadCardData();
});

```

In html view [index.html | Click here](https://github.com/HappyRogelio7/Draggable-Cards-in-web/tree/master/index.html)



---

## Aditional Information

[My website](https://happyrogelio7.xyz), My website

[Discord](https://discord.gg/3EebYUyeUX), Support My Server Discord

[Kaory Studios](https://kaorystudios.xyz), Website Kaory Studios

[Kaory Studios Discord](https://discord.gg/Gw7m8kC), Support Kaory Studios

---

© Copyright HappyRogelio7 2017-2023 ©
RIGHTS RESERVED

## Special Thanks

- **Visual Studio Code**: [Link](https://code.visualstudio.com/)
  ![Visual Studio Code Logo](./statics/imgs/vscode.png)
  - Description: Visual Studio Code is a lightweight and powerful source code editor that provides excellent support for various programming languages, debugging, and version control integration.

Happy coding!