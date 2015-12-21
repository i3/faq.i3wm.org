/**
 * @constructor
 * just a span with content
 * useful for subclassing
 */
var SimpleContent = function () {
    WrappedElement.call(this);
    this._content = '';
};
inherits(SimpleContent, WrappedElement);

SimpleContent.prototype.setContent = function (text) {
    this._content = text;
    if (this._element) {
        this._element.html(text);
    }
};

SimpleContent.prototype.getContent = function () {
    return this._content;
};

SimpleContent.prototype.createDom = function () {
    this._element = this.makeElement('span');
    this._element.html(this._content);
};
