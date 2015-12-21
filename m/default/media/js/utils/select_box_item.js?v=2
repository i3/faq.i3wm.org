/**
 * @constructor
 * an item used for the select box described below
 */
var SelectBoxItem = function () {
    Widget.call(this);
    this._id = null;
    this._name = null;
    this._description = null;
    this._content_class = BoxItemContent;//default expects a single text node
    //content element - instance of this._content_class
    this._content = undefined;
    this._selector = undefined;//the selector object
};
inherits(SelectBoxItem, Widget);

SelectBoxItem.prototype.setId = function (id) {
    this._id = id;
    if (this._element) {
        this._element.data('itemId', id);
    }
};

SelectBoxItem.prototype.getId = function () {
    return this._id;
};

SelectBoxItem.prototype.setName = function (name) {
    this._name = name;
    if (this._content) {
        this._content.setName(name);
    }
};

SelectBoxItem.prototype.setDescription = function (description) {
    this._description = description;
    if (this._element) {
        this._element.data('originalTitle');
    }
};

SelectBoxItem.prototype.getData = function () {
    //todo: stuck using old key names, change after merge
    //with the user-groups branch
    return {
        id: this._id,
        title: this._name,
        details: this._description
    };
};

SelectBoxItem.prototype.setSelector = function (sel) {
    this._selector = sel;
};

SelectBoxItem.prototype.getSelector = function (sel) {
    return this._selector;
};

SelectBoxItem.prototype.getContent = function () {
    return this._content;
};

SelectBoxItem.prototype.isSelected = function () {
    return this._element.hasClass('selected');
};

SelectBoxItem.prototype.setSelected = function (is_selected) {
    if (is_selected) {
        this._element.addClass('selected');
    } else {
        this._element.removeClass('selected');
    }
};

SelectBoxItem.prototype.detach = function () {
    this._element.detach();
};

SelectBoxItem.prototype.createDom = function () {
    var elem = this.makeElement('li');
    this._element = elem;
    elem.data('itemId', this._id);
    elem.data('itemOriginalTitle', this._description);
    var content = new this._content_class();
    content.setName(this._name);
    elem.append(content.getElement());
    this._content = content;
};
/**
 * this method sets css class to the item's DOM element
 */
SelectBoxItem.prototype.addCssClass = function (css_class) {
    this._element.addClass(css_class);
};

SelectBoxItem.prototype.decorate = function (element) {
    this._element = element;
    //set id and description
    this._id = element.data('itemId');
    this._description = element.data('originalTitle');

    //work on setting name
    var content_source = element.contents().detach();
    var content = new this._content_class();
    //assume that we want first node only
    content.setContent(content_source[0]);
    this._content = content;
    this._name = content.getName();//allows to abstract from structure

    this._element.append(content.getElement());
};
