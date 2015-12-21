/**
 * @contsructor
 * a form helper that disables submit button
 * after it is submitted the first time
 * to prevent double submits
 */
var OneShotForm = function () {
    WrappedElement.call(this);
    this._submitBtn = undefined;
};
inherits(OneShotForm, WrappedElement);

OneShotForm.prototype.setSubmitButton = function (button) {
    this._submitBtn = button;
};

OneShotForm.prototype.enable = function () {
    this._element.data('submitted', false);
    this._submitBtn.removeClass('disabled');
};

OneShotForm.prototype.disable = function () {
    this._element.data('submitted', true);
    this._submitBtn.addClass('disabled');
};

OneShotForm.prototype.decorate = function (element) {
    this._element = element;
    var me = this;
    var button = this._submitBtn;
    var disabler = function (evt) {
        if (element.data('submitted') === true) {
            evt.preventDefault();
        } else {
            me.disable();
            return true;
        }
    };
    element.submit(disabler);
};
