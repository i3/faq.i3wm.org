/**
 * @constructor
 * a simple link
 */
var Link = function () {
    WrappedElement.call(this);
};
inherits(Link, WrappedElement);

Link.prototype.setUrl = function (url) {
    this._url = url;
};

Link.prototype.setText = function (text) {
    this._text = text;
};

Link.prototype.createDom = function () {
    var link = this.makeElement('a');
    this._element = link;
    link.attr('href', this._url);
    link.html(this._text);
};
