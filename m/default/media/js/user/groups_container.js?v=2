/**
 * @constructor
 */
var GroupsContainer = function () {
    WrappedElement.call(this);
};
inherits(GroupsContainer, WrappedElement);

GroupsContainer.prototype.decorate = function (element) {
    this._element = element;
    var groups = [];
    var group_names = [];
    var me = this;
    //collect list of groups
    $.each(element.find('tr'), function (idx, li) {
        var group = new UserGroup();
        group.setGroupsContainer(me);
        group.decorate($(li));
        groups.push(group);
        group_names.push(group.getName());
    });
    this._groups = groups;
    this._group_names = group_names;
};

GroupsContainer.prototype.addGroup = function (group_data) {
    var group_name = group_data.name;
    if ($.inArray(group_name, this._group_names) > -1) {
        return;
    }
    var group = new UserGroup(group_name);
    group.setContent(group_data.html);
    group.setGroupsContainer(this);
    this._groups.push(group);
    this._group_names.push(group_name);
    this._element.append(group.getElement());
};

GroupsContainer.prototype.removeGroup = function (group) {
    var idx = $.inArray(group, this._groups);
    if (idx === -1) {
        return;
    }
    this._groups.splice(idx, 1);
    this._group_names.splice(idx, 1);
    group.dispose();
};
