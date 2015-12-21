var DeleteIcon = function (title) {
    SimpleControl.call(this);
    this._title = title;
    this._content = null;
};
inherits(DeleteIcon, SimpleControl);

DeleteIcon.prototype.decorate = function (element) {
    this._element = element;
    this._element.attr('class', 'js-delete-icon');
    this._element.attr('title', this._title);
    if (this._handler !== null) {
        this.setHandlerInternal();
    }
};

DeleteIcon.prototype.setHandlerInternal = function () {
    setupButtonEventHandlers(this._element, this._handler);
};

DeleteIcon.prototype.createDom = function () {
    this._element = this.makeElement('span');
    this.decorate(this._element);
    if (this._content !== null) {
        this.setContent(this._content);
    }
};

DeleteIcon.prototype.setContent = function (content) {
    if (this._element === null) {
        this._content = content;
    } else {
        this._content = content;
        this._element.html(content);
    }
};
