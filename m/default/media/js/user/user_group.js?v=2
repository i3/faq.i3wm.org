/**
 * @constructor
 * @param {string} name
 */
var UserGroup = function (name) {
    WrappedElement.call(this);
    this._name = name;
    this._content = null;
};
inherits(UserGroup, WrappedElement);

UserGroup.prototype.getDeleteHandler = function () {
    var group_name = this._name;
    var me = this;
    var groups_container = me._groups_container;
    return function () {
        var data = {
            user_id: askbot.data.viewUserId,
            group_name: group_name,
            action: 'remove'
        };
        $.ajax({
            type: 'POST',
            dataType: 'json',
            data: data,
            cache: false,
            url: askbot.urls.edit_group_membership,
            success: function () {
                groups_container.removeGroup(me);
            }
        });
    };
};

UserGroup.prototype.setContent = function (content) {
    this._content = content;
};

UserGroup.prototype.getName = function () {
    return this._name;
};

UserGroup.prototype.setGroupsContainer = function (container) {
    this._groups_container = container;
};

UserGroup.prototype.decorate = function (element) {
    this._element = element;
    this._name = $.trim(element.find('a').html());
    var deleter = new DeleteIcon();
    deleter.setHandler(this.getDeleteHandler());
    //deleter.setContent(gettext('Remove'));
    this._element.find('td:last').append(deleter.getElement());
    this._delete_icon = deleter;
};

UserGroup.prototype.createDom = function () {
    var element = this.makeElement('tr');
    element.html(this._content);
    this._element = element;
    this.decorate(element);
};

UserGroup.prototype.dispose = function () {
    this._delete_icon.dispose();
    this._element.remove();
};
