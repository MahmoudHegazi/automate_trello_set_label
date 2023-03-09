# automate_trello_set_label
this javascript can be added to the console to set the label to trello list,

![image](https://user-images.githubusercontent.com/55125302/222796560-1a665d91-cea3-4c62-aec9-bc68f789815a.png)


```javascript
function markListLabelsByLabelName(targetLabel = 'Milestone 1', listIndex = 0, speed = 2000) {
    /* for only not labeled cards*/
    if (speed < 2000) {
        console.log("min speed is 2000");
        return false;
    }

    let timeouts = 0;
    let automationI = 0;
    let targetList = null;
    let foundAtLeastCard = false;
    if (listIndex < $('.js-list.list-wrapper').length && $('.js-list.list-wrapper').eq(listIndex).length) {
        targetList = $('.js-list.list-wrapper').eq(listIndex);
    } else {
        console.log(`you do not have enogh lists please set valid listIndex starting from 0 to first list, current Lists: ${$('.js-list.list-wrapper').length}`);
        return false;
    }

    const allLabels = $(targetList).find(".list-card.js-member-droppable.ui-droppable.ui-sortable-handle");
    $(allLabels).each((i, elm) => {
        if (!$(elm).find(".js-list-card-front-labels-container .ef93UiHgHz1OFw").length) {
            if (foundAtLeastCard == false) {
                console.log("this cards in the list not have a label, script will automate them.");
                foundAtLeastCard = true;
            }

            console.log(`${i} not have a label`);
            if ($(elm).find(".list-card-operation").length) {
                timeouts += speed;
                setTimeout(() => {
                    automationI += 1;
                    const targetElm = $(elm).find(".list-card-operation");
                    if ($(targetElm).length) {
                        $(targetElm).click();
                        const editBtn = $(".quick-card-editor a.quick-card-editor-buttons-item.js-edit-labels");
                        if ($(editBtn).length) {
                            $(editBtn).click();
                            setTimeout(() => {
                                const currentLabels = $("section[data-testid='labels-popover-labels-screen'] ul li");
                                $(currentLabels).each((ei, labelLi) => {
                                    const targetLabelELm = $(labelLi).find("[data-testid='card-label']");
                                    if ($(targetLabelELm).length &&
                                        ($(targetLabelELm).text().trim() == targetLabel.trim())) {
                                        console.log(`found label for card index: ${i}`);
                                        const liParent = $(targetLabelELm).closest("li");
                                        if ($(liParent).length && !$(liParent).find("span.P98k_n1VEYvMVQ").length && $(liParent).find("span svg").length) {
                                            const checkBoxSpan = $(liParent).find("span.CpyGgjAzUkQDno");
                                            $(checkBoxSpan).click();
                                            if ($("section[data-testid='labels-popover-labels-screen'] button[aria-label='Close popover']").length) {

                                                $("section[data-testid='labels-popover-labels-screen'] button[aria-label='Close popover']").click();
                                            }
                                        }

                                    }

                                });

                            }, 1000);

                        }
                    }
                }, timeouts);


            }
        }
    });
    setTimeout(() => {

        if ($(".quick-card-editor-card input[type='submit']").length) {
            $(".quick-card-editor-card input[type='submit']").click();
        }
        if (foundAtLeastCard != true) {
            console.log('no cards proccessed');
        }
        console.log('completed automation proccess');
    }, timeouts + 1200);
}
```
