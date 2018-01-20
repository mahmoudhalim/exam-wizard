# exam-wizard
Exam Wizard Jquery Plugin to Handling Examinations, Questions, Answers Easily, its helpful to build exams,  surveys, questioners 

## Screenshot from Simple Demo

![Yii2 timeDownCounter screenshot_t1](http://2nees.com/github/examWizard/examwizard.png)

## Features

1. Ability to build unlimited of questions with full of action control.
2. Ability to let user marks questions.
3. Ability to let users use quick access menu.
4. Ability to set real answer or alternate answer to display in quick access menu.
5. Ability to determind number of qustions in quick access menu.
6. Ability to display summery of result on modal when user click on finish button.
7. Ability to enable/disable mark questions, quick access, finish modal.
8. Ability to update selectors for actions control.
9. Ability to trigger callback function on every main action.
10. Ability to prevent next/prev button dependance on pre-trigger-funtion.
11. Ability To Save All Users Answers in cookies.
12. Ability to add auth key for cookie to keep data safe for every user/exam dependance of your rules.
13. Ability to retrieve all marked value from examVariable directly.
14. Ability to retrieve all answerd value from examVariable directly with options to retrive all form data or just questions data, also we can get data sorted as a groub or with normal array.
15. Ability to retrieve total of marked questions from examVariable directly.
16. Ability to retrieve total of answerd questions from examVariable directly.
17. Ability to retrieve total of Remaining questions from examVariable directly.
18. Ability to retrieve current question number from examVariable directly.
19. You will work on exam wizard with data-attribute for name, values and types, and this give you a power to keep real name, value and type without change, its helpfual if you like to use it in exsisting form, with fremworks, or cms.
20. This plugin work on input type, select and textarea.
21. This plugin work on multiple value for select and checkbox fields.

## Dependency
1. jQuery Library

## Installation:
The preferred way to install this extension is through [composer](https://getcomposer.org/).

Either run

`php composer.phar require --prefer-dist aneeshikmat/exam-wizard "*@dev"`

## Usage
To use this plugin you need to write your code like this template(in basic use):

```
<!-- Html Template -->
<form id="examwizard-question"><!-- examwizard-question #id selector for exam form(you can update it by option -->
  <div class="yourClass" data-question="1">
    ------
    <input type="radio" data-alternatetype="radio" name="fieldName[0]" data-alternateName="answer[0]" data-alternateValue="A" value="1"/>
    ------
  </div>
    <div class="yourClass hidden" data-question="2"><!-- We Add hidden class here -->
    ------
    <input type="radio" data-alternatetype="radio" name="fieldName[1]" data-alternateName="answer[1]" data-alternateValue="A" value="1"/>
    ------
  </div>
  
  <!-- Hidden field, save current question number, marked values, total of question -->
  <input type="hidden" value="1" id="currentQuestionNumber" name="currentQuestionNumber" />
  <input type="hidden" value="18" id="totalOfQuestion" name="totalOfQuestion" />
  <input type="hidden" value="[]" id="markedQuestion" name="markedQuestions" />

</form>

<!-- Scripts -->
<script src="js/jquery.js"></script><!-- Required -->
<script src="js/examwizard.min.js"></script><!-- Required -->

<script>
    var examWizard = $.fn.examWizard();
</script>
```

As you see, its very simple, and now we will be explaning this code, and then go to display all option may be use to help us.
1) Form tag must be has an #id selector, the default selector is examwizard-question, and you can change it by set it in examwizard configration like this: 
```
<script>
    var examWizard = $.fn.examWizard({
      formSelector: "#new-examwizard-question",
    });
</script>
```
2) Wrapper <div> has an data-question attribute with question number value.
3) Question Field most be write with this rule:
  
       a. data-alternatetype: this data attribute has an field type.(required).
  
       b. data-alternateName: this data attribute has an alternate name, this is useful to keep field name without any update, its useful if you need to update old form, use in cms or fremwork, alternateName most has value like this rule "answer[question-number - 1]" so that if we on question 1 thats mean alternateName="answerd[0]".(required)
  
       c. data-alternateValue: this data attribute has an alternate value for field, its useful on quick access menu, its will be display a readable value for user, for example if field value is 1 and data-alternateValue="A" then the displayed value will be is "A" not "1".(Optional)
  
4) #currentQuestionNumber field is an hidden field save current question number, init value = 1.
5) #totalOfQuestion field is an hidden field save total of question number, its most set to can be groubing question. (in this example we has an 18 question, so that exam-wizard will handling 18 question only.
6) #markedQuestion field is an hidden field save all marked value, default value is empty array "[]". (if you disabled marks option, you dont need this field).
  
### Quick Access
Quick access section its an simple layout useful to display answers input by users and marked links, this option is enabled by default.
```
<!-- Quick Access Html Template -->
<div class="col-xs-3" id="quick-access-section">
  <table> <!-- You can use table, or any other html tag like div, spans..etc -->
    <tr class="question-response-rows" data-question="1">
        <td>1</td>
        <td class="question-response-rows-value">-</td>
    </tr>
    <tr class="question-response-rows" data-question="2">
        <td>2</td>
        <td class="question-response-rows-value">-</td>
    </tr>
  </table>
  <div class="col-xs-12">
      <a href="javascript:void(0)" class="btn btn-success" id="quick-access-prev">< Back</a>
      <span class="alert alert-info" id="quick-access-info"></span>
      <a href="javascript:void(0)" class="btn btn-success" id="quick-access-next">Next ></a>
  </div>
</div>
```

The main item in quick Access is:

A) #quick-access-section in div wrapper.
B) .question-response-rows this class required in every row in quick access, and data-question is required too, its has an question number.

C) .question-response-rows-value this class has an value for each question, defualt value when nothing select is "-".

D) .quick-access-prev is quick access class to do back action if we groubing rows(like screen shot)

E) .quick-access-next is quick access class to do next action if we groubing rows(like screen shot)

F) #quick-access-info This selector will has an information about quick access page number of total like 1/2(look at screen shot).


And now, let us see full configration for quickaccess
```
var examWizard = $.fn.examWizard({
    quickAccessOption: {
      enableAccessSection:    true,// Enable or disable Quick Access
      quickAccessPagerItem:   'Full',// Number of rows in quick access, by default is full and you can send your number like 9
      quickAccessInfoSeperator:'/',// the sperator for quick access information the default is /. (1/2)
      quickAccessDefaultRowVal:'-', // Default Row Value when value is empty
      quickAccessRowValSeparator: ', ', // Default Value Sperator for multible value
      nextCallBack            :function(){}, // CallBack Function when click on next quick access callBack
      prevCallBack            :function(){}, // CallBack Function when click on prev quick access callBack
    }
});
```

### Marked Buttons
Its an button used to display marked link on quick access, its easy way to let user to back to question if he like to update, or answer.

```
<!-- Mark Button Template -->

<div class="mark-unmark-wrapper" data-question="1">
    <a href="javascript:void(0);" class="mark-question btn btn-success" data-question="1">
        <b>MARK</b>
    </a>
    <a href="javascript:void(0);" class="hidden unmark-question btn btn-success" data-question="1">
        <b>UNMARK</b>
    </a>
</div>
<div class="mark-unmark-wrapper hidden" data-question="2">
    <a href="javascript:void(0);" class="mark-question btn btn-success" data-question="2">
        <b>MARK</b>
    </a>
    <a href="javascript:void(0);" class="hidden unmark-question btn btn-success" data-question="2">
        <b>UNMARK</b>
    </a>
</div>

```
A) .mark-unmark-wrapper is the main selector wrapper for mark/unmark button, data-question has an question number.(other wrapper will contaion hidden class).

B) .mark-question is the selector for trigger mark action, data-question has an question number.

C) .unmark-question is the selector for trigger unmark action, data-question has an question number. (by default will has hidden class).

And This is JS Option:

```
var examWizard = $.fn.examWizard({
    markOption: {
        markedLabel:            'Marked',// This is Link lable will be display on quick access (look to screen shot).
        enableMarked:           true,// Enalbe or Disable mark button, by default its enable
        markCallBack:           function(){},// CallBack Function when click on mark button
        unMarkCallBack:         function(){},// CallBack Function when click on unmark button
    },
});
```

