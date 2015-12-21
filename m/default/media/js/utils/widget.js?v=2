/**
 * @constructor
 * Widget is a Wrapped element with state
 */
var Widget = function () {
    WrappedElement.call(this);
    this._state = undefined;
};
inherits(Widget, WrappedElement);

Widget.prototype.setState = function (state) {
    this._state = state;
};

Widget.prototype.getState = function () {
    return this._state;
};

Widget.prototype.makeButton = function (label, handler) {
    var button = this.makeElement('button');
    button.html(label);
    setupButtonEventHandlers(button, handler);
    return button;
};
