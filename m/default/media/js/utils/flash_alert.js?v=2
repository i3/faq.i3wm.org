/**
 * creates an alert that will momentarily flash
 * and then self-destruct
 */
var FlashAlert = function (text) {
    WrappedElement.call(this);
    this._text = text;
};
inherits(FlashAlert, WrappedElement);

FlashAlert.prototype.setPostRunHandler = function (handler) {
    this._postRunHandler = handler;
};

FlashAlert.prototype.setText = function (text) {
    this._text = text;
    if (this.hasElement()) {
        this._element.html(text);
    }
};

FlashAlert.prototype.run = function () {
    var element = this._element;
    var me = this;
    var postRunHandler = this._postRunHandler;
    var finish = function () {
        element.fadeOut();
        me.dispose();
        if (postRunHandler) {
            postRunHandler();
        }
    };
    element.fadeIn(function () { setTimeout(finish, 1000); });
};

FlashAlert.prototype.createDom = function () {
    var element = this.makeElement('div');
    element.addClass('flash-alert');
    element.hide();
    this._element = element;
    if (this._text) {
        element.html(this._text);
    }
};
