var GroupAdderWidget = function () {
    WrappedElement.call(this);
    this._state = 'display';//display or edit
};
inherits(GroupAdderWidget, WrappedElement);

/**
 * @param {string} state
 */
GroupAdderWidget.prototype.setState = function (state) {
    if (state === 'display') {
        this._element.html(gettext('add group'));
        this._input.hide();
        this._input.val('');
        this._button.hide();
    } else if (state === 'edit') {
        this._element.html(gettext('cancel'));
        this._input.show();
        this._input.focus();
        this._button.show();
    } else {
        return;
    }
    this._state = state;
};

GroupAdderWidget.prototype.getValue = function () {
    return this._input.val();
};

GroupAdderWidget.prototype.addGroup = function (group_data) {
    this._groups_container.addGroup(group_data);
};

GroupAdderWidget.prototype.getAddGroupHandler = function () {
    var me = this;
    return function () {
        var group_name = me.getValue();
        var data = {
            group_name: group_name,
            user_id: askbot.data.viewUserId,
            action: 'add'
        };
        $.ajax({
            type: 'POST',
            dataType: 'json',
            data: data,
            cache: false,
            url: askbot.urls.edit_group_membership,
            success: function (data) {
                if (data.success) {
                    me.addGroup(data);
                    me.setState('display');
                } else {
                    var message = data.message;
                    showMessage(me.getElement(), message, 'after');
                }
            }
        });
    };
};

GroupAdderWidget.prototype.setGroupsContainer = function (container) {
    this._groups_container = container;
};

GroupAdderWidget.prototype.toggleState = function () {
    if (this._state === 'display') {
        this.setState('edit');
    } else if (this._state === 'edit') {
        this.setState('display');
    }
};

GroupAdderWidget.prototype.decorate = function (element) {
    this._element = element;
    var input = this.makeElement('input');
    input.attr('type', 'text');
    this._input = input;

    var groupsAc = new AutoCompleter({
        url: askbot.urls.getGroupsList,
        minChars: 1,
        useCache: false,
        matchInside: false,
        maxCacheLength: 100,
        delay: 10
    });
    groupsAc.decorate(input);

    var button = this.makeElement('button');
    button.html(gettext('add'));
    this._button = button;
    element.before(input);
    input.after(button);
    this.setState('display');
    setupButtonEventHandlers(button, this.getAddGroupHandler());
    var me = this;
    setupButtonEventHandlers(
        element,
        function () { me.toggleState(); }
    );
};
