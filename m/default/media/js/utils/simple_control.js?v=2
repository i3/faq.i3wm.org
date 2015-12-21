var SimpleControl = function () {
    WrappedElement.call(this);
    this._handler = null;
    this._title = null;
};
inherits(SimpleControl, WrappedElement);

SimpleControl.prototype.setHandler = function (handler) {
    this._handler = handler;
    if (this.hasElement()) {
        this.setHandlerInternal();
    }
};

SimpleControl.prototype.getHandler = function () {
    return this._handler;
};

SimpleControl.prototype.setHandlerInternal = function () {
    //default internal setHandler behavior
    setupButtonEventHandlers(this._element, this._handler);
};

SimpleControl.prototype.setTitle = function (title) {
    this._title = title;
};

SimpleControl.prototype.decorate = function (element) {
    this._element = element;
    this.setHandlerInternal();
};
