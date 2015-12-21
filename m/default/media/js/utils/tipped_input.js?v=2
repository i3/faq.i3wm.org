/**
 * Can be used for an input box or textarea.
 * The original value will be treated as an instruction.
 * When user focuses on the field, the tip will be gone,
 * when the user escapes without typing anything besides
 * perhaps empty text, the instruction is restored.
 * When instruction is shown, class "blank" is present
 * in the input/textare element.
 *
 * For the usage examples - search for "new TippedInput"
 * there is at least one example for both - decoration and
 * the "dom creation" patterns.
 *
 * Also - in the FileUploadDialog the TippedInput is used
 * as a sub-element - the widget composition use case.
 */
var TippedInput = function () {
    /* the call below is part 1 of the inheritance pattern */
    WrappedElement.call(this);
    this._instruction = null;
    this._attrs = {};
    //this._is_one_shot = false;//if true on starting typing effect is gone
};
inherits(TippedInput, WrappedElement);
/* the line above is part 2 of the inheritance pattern
 see definition of the function "inherits" for more details
*/

/* Below are all the custom methods of the
  TippedInput class, as well as some required functions
*/

TippedInput.prototype.reset = function () {
    $(this._element).val(this._instruction);
    $(this._element).addClass('blank');
};

/*TippedInput.prototype.setIsOneShot = function (boolValue) {
    this._is_one_shot = boolValue;
};*/

TippedInput.prototype.setInstruction = function (text) {
    this._instruction = text;
};

TippedInput.prototype.setAttr = function (key, value) {
    this._attrs[key] = value;
};

TippedInput.prototype.isBlank = function () {
    return this.getVal() === this._instruction;
};

TippedInput.prototype.getVal = function () {
    return this._element.val();
};

TippedInput.prototype.setVal = function (value) {
    if (value) {
        this._element.val(value);
        if (this.isBlank()) {
            this._element.addClass('blank');
        } else {
            this._element.removeClass('blank');
        }
    }
};
/**
 * Creates the DOM of tipped input from scratch
 * Notice that there is also a "decorate" method.
 * At least one - createDom or decorate is required,
 * depending on the usage.
 */
TippedInput.prototype.createDom = function () {
    this._element = this.makeElement('input');
    var element = this._element;
    element.val(this._instruction);

    //here we re-use the decorate call (next method)
    //to avoid copy-pasting code
    this.decorate(element);
};

/**
 * Attaches the TippedInput behavior to
 * a pre-existing <input> element
 *
 * decorate() method normally does not create
 * new dom elements, but it might add some missing elements,
 * if necessary.
 *
 * for example the decorate might be composing inside
 * a more complex widget, in which case other elements
 * can be added via a "composition" pattern, or
 * just "naked dom elements" added to the current widget's element
 *
 */
TippedInput.prototype.decorate = function (element) {
    this._element = element;//mandatory line

    //part 1 - initialize some values and create dom
    element.attr(this._attrs);

    var instruction_text = this.getVal();
    this._instruction = instruction_text;
    this.reset();
    var me = this;

    //part 2 - attach event handlers
    $(element).focus(function () {
        if (me.isBlank()) {
            $(element)
                .val('')
                .removeClass('blank');
        }
    });
    $(element).blur(function () {
        var val = $(element).val();
        if ($.trim(val) === '') {
            $(element)
                .val(instruction_text)
                .addClass('blank');
        }
    });
    $(element).keydown(
        makeKeyHandler(27, function () {
            $(element).blur();
        })
    );
};
