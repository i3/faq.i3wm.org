/**
 * A list of items from where one can be selected
 */
var SelectBox = function () {
    Widget.call(this);
    this._items = [];
    this._select_handler = function () {};//empty default
    this._is_editable = false;
    this._item_class = SelectBoxItem;
};
inherits(SelectBox, Widget);

SelectBox.prototype.setItemClass = function (itemClass) {
    this._item_class = itemClass;
};

SelectBox.prototype.setEditable = function (is_editable) {
    this._is_editable = is_editable;
};

SelectBox.prototype.isEditable = function () {
    return this._is_editable;
};

SelectBox.prototype.detachAllItems = function () {
    var items = this._items;
    $.each(items, function (idx, item) {
        item.detach();
    });
    this._items = [];
};

SelectBox.prototype.getItem = function (id) {
    var items = this._items;
    for (var i = 0; i < items.length; i++) {
        if (items[i].getId() === id) {
            return items[i];
        }
    }
    return undefined;
};

SelectBox.prototype.getItemByIndex = function (idx) {
    return this._items[idx];
};

/**
 * removes all items
 */
SelectBox.prototype.empty = function () {
    var items = this._items;
    $.each(items, function (idx, item) {
        item.dispose();
    });
    this._items = [];
};

/*
 * why do we have these two almost identical methods?
 * the difference seems to be remove/vs fade out
 */
SelectBox.prototype.removeItem = function (id) {
    var item = this.getItem(id);
    item.getElement().fadeOut();
    item.dispose();
    var idx = $.inArray(item, this._items);
    if (idx !== -1) {
        this._items.splice(idx, 1);
    }
};
SelectBox.prototype.deleteItem = function (id) {
    var item = this.getItem(id);
    if (item === undefined) {
        return;
    }
    item.dispose();
    var idx = $.inArray(item, this._items);
    if (idx !== -1) {
        this._items.splice(idx, 1);
    }
};

SelectBox.prototype.createItem = function () {
    return new this._item_class();
};

SelectBox.prototype.getItemIndex = function (item) {
    var idx = $.inArray(item, this._items);
    if (idx === -1) {
        throw 'index error';
    }
    return idx;
};

SelectBox.prototype.addItemObject = function (item) {
    this._items.push(item);
    this._element.append(item.getElement());
    this.selectItem(item);
    item.setSelector(this);
    //set event handler
    var me = this;
    setupButtonEventHandlers(
        item.getElement(),
        me.getSelectHandler(item)
    );
};

/** @todo: rename to setItem?? have a problem when id's are all say 0 */
SelectBox.prototype.addItem = function (id, name, description) {

    if (this.hasElement() === false) {
        return;
    }
    //delete old item
    this.deleteItem(id);
    //create new item
    var item = this.createItem();
    item.setId(id);
    item.setName(name);
    item.setDescription(description);
    //add item to the SelectBox
    this.addItemObject(item);

    return item;
};

SelectBox.prototype.getSelectedItem = function () {
    for (var i = 0; i < this._items.length; i++) {
        var item = this._items[i];
        if (item.isSelected()) {
            return item;
        }
    }
    return undefined;
};

SelectBox.prototype.getSelectedItemData = function () {
    var item = this.getSelectedItem();
    if (item) {
        return item.getData() || undefined;
    } else {
        return undefined;
    }
};

SelectBox.prototype.selectItem = function (item) {
    this.clearSelection();
    item.setSelected(true);
};

SelectBox.prototype.clearSelection = function () {
    $.each(this._items, function (idx, item) {
        item.setSelected(false);
    });
};

SelectBox.prototype.setSelectHandler = function (handler) {
    this._select_handler = handler;
};

SelectBox.prototype.getSelectHandlerInternal = function () {
    return this._select_handler;
};

SelectBox.prototype.getSelectHandler = function (item) {
    var me = this;
    return function () {
        me.selectItem(item);
        var handler = me.getSelectHandlerInternal();
        handler(item.getData());
    };
};

SelectBox.prototype.decorate = function (element) {
    this._element = element;
    var me = this;
    var box_items = this._items;
    var item_elements = this._element.find('.select-box-item');
    item_elements.each(function (idx, item_element) {
        var item = me.createItem();
        item.decorate($(item_element));
        box_items.push(item);
        setupButtonEventHandlers(
            item.getElement(),
            me.getSelectHandler(item)
        );
    });
};

SelectBox.prototype.createDom = function () {
    var element = this.makeElement('ul');
    this._element = element;
    element.addClass('select-box');
};
