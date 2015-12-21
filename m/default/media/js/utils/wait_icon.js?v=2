/**
 * @constructor
 * a loader
 */
var WaitIcon = function () {
    WrappedElement.call(this);
    this._isVisible = false;
};
inherits(WaitIcon, WrappedElement);

WaitIcon.prototype.setVisible = function (isVisible) {
    this._isVisible = isVisible;
    if (this._element) {
        if (this._isVisible === true) {
            this._element.show();
        } else {
            this._element.hide();
        }
    }
};

WaitIcon.prototype.hide = function () {
    this.setVisible(false);
};

WaitIcon.prototype.show = function () {
    this.setVisible(true);
};

WaitIcon.prototype.createDom = function () {
    var box = this.makeElement('div');
    box.addClass('wait-icon-box');
    this._element = box;
    var img = this.makeElement('img');
    img.attr('src', mediaUrl('media/images/ajax-loader.gif'));
    box.append(img);
    this.setVisible(this._isVisible);
};
