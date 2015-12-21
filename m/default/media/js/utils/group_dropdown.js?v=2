/**
 * This is a dropdown list elment
 */
var GroupDropdown = function (groups) {
    WrappedElement.call(this);
    this._group_list = groups;
};
inherits(GroupDropdown, WrappedElement);

GroupDropdown.prototype.createDom =  function () {
    this._element = this.makeElement('ul');
    this._element.attr('class', 'dropdown-menu');
    this._element.attr('id', 'groups-dropdown');
    this._element.attr('role', 'menu');
    this._element.attr('aria-labelledby', 'navGroups');

    this._input_box = new TippedInput();
    this._input_box.setInstruction(gettext('group name'));
    this._input_box.createDom();
    this._input_box_element = this._input_box.getElement();
    this._input_box_element.attr('class', 'group-name');
    this._input_box_element.hide();
    this._add_link = this.makeElement('a');
    this._add_link.attr('href', '#');
    this._add_link.attr('class', 'group-name');
    this._add_link.text(gettext('add new group'));

    for (var i = 0; i < this._group_list.length; i++) {
        var li = this.makeElement('li');
        var a = this.makeElement('a');
        a.text(this._group_list[i].name);
        a.attr('href', this._group_list[i].link);
        a.attr('class', 'group-name');
        li.append(a);
        this._element.append(li);
    }
    if (askbot.data.userIsAdmin) {
        this.enableAddGroups();
    }
};

/**
 * inserts a link to group with a given url to the group page
 * and name
 */
GroupDropdown.prototype.insertGroup = function (group_name, url) {

    //1) take out first and last list elements:
    // everyone and the "add group" item
    var list = this._element.children();
    var everyoneGroup = list.first().detach();
    var groupAdder = list.last().detach();
    var divider = this._element.find('.divider').detach();

    //2) append group link into the list
    var li = this.makeElement('li');
    var a = this.makeElement('a');
    a.attr('href', url);
    a.attr('class', 'group-name');
    a.text(group_name);
    li.append(a);
    li.hide();
    this._element.append(li);

    //3) sort rest of the list alphanumerically
    sortChildNodes(
        this._element,
        function (a, b) {
            var valA = $(a).find('a').text().toLowerCase();
            var valB = $(b).find('a').text().toLowerCase();
            return (valA < valB) ? -1 : (valA > valB) ? 1 : 0;
        }
    );

    //a dramatic effect
    li.fadeIn();

    //4) reinsert the first and last elements of the list:
    this._element.prepend(everyoneGroup);
    this._element.append(divider);
    this._element.append(groupAdder);
};

GroupDropdown.prototype.setState = function (state) {
    if (state === 'display') {
        this._input_box_element.hide();
        this._add_link.show();
    }
};

GroupDropdown.prototype.hasGroup = function (groupName) {
    var items = this._element.find('li');
    for (var i = 1; i < items.length - 1; i++) {
        var cGroupName = $(items[i]).find('a').text();
        if (cGroupName.toLowerCase() === groupName.toLowerCase()) {
            return true;
        }
    }
    return false;
};

GroupDropdown.prototype._add_group_handler = function () {
    var group_name = this._input_box_element.val();
    var me = this;
    if (!group_name) {
        return;
    }

    $.ajax({
        type: 'POST',
        url: askbot.urls.add_group,
        data: {group: group_name},
        success: function (data) {
            if (data.success) {
                var groupName = data.group_name;
                if (me.hasGroup(groupName)) {
                    var message = interpolate(gettext(
                            'Group %(name)s already exists. Group names are case-insensitive.'
                        ), {'name': groupName}, true
                    );
                    notify.show(message);
                    return false;
                } else {
                    me.insertGroup(data.group_name, data.url);
                    me.setState('display');
                    return true;
                }
            } else {
                notify.show(data.message);
                return false;
            }
        },
        error: function () {console.log('error');}
    });
};

GroupDropdown.prototype.enableAddGroups = function () {
    var self = this;
    this._add_link.click(function () {
        self._add_link.hide();
        self._input_box_element.show();
        self._input_box_element.focus();
    });
    this._input_box_element.keydown(function (event) {
        if (event.which === 13 || event.keyCode === 13) {
            self._add_group_handler();
            self._input_box_element.val('');
        }
    });

    var divider = this.makeElement('li');
    divider.attr('class', 'divider');
    this._element.append(divider);

    var container = this.makeElement('li');
    container.append(this._add_link);
    container.append(this._input_box_element);

    this._element.append(container);
};
