var EditLink = function () {
    SimpleControl.call(this);
};
inherits(EditLink, SimpleControl);

EditLink.prototype.createDom = function () {
    var element = $('<a></a>');
    element.addClass('edit js-edit');
    this.decorate(element);
};

EditLink.prototype.decorate = function (element) {
    this._element = element;
    this._element.attr('title', gettext('click to edit this comment'));
    this._element.html(gettext('edit'));
    this.setHandlerInternal();
};
