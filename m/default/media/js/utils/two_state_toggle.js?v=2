/**
 * A button on which user can click
 * and change status with regard to certain item (follow/unfollow something,
 * join/leave group, or toggle some state)
 * The button has four states on-prompt, off-prompt, on-state and off-state
 * on-prompt is activated on mouseover, when user is not part of group
 * off-prompt - on mouseover, when user is part of group
 * on-state - when user is part of group and mouse is not over the button
 * off-state - same as above, but when user is not part of the group
 */
var TwoStateToggle = function () {
    SimpleControl.call(this);
    this._state = null;
    this._messages = {};
    this._states = [
        'on-state',
        'off-state',
        'on-prompt',
        'off-prompt'
    ];
    this._handler = this.getDefaultHandler();
    this._postData = {};
    this.toggleUrl = '';//public property
    this.setupDefaultDataValidators();
};
inherits(TwoStateToggle, SimpleControl);

TwoStateToggle.prototype.setPostData = function (data) {
    this._postData = data;
};

TwoStateToggle.prototype.getPostData = function () {
    return this._postData;
};

TwoStateToggle.prototype.resetStyles = function () {
    var element = this._element;
    var states = this._states;
    $.each(states, function (idx, state) {
        element.removeClass(state);
    });
    this.setText('');
};

TwoStateToggle.prototype.isOn = function () {
    return this._element.data('isOn');
};

TwoStateToggle.prototype.setupDefaultDataValidators = function () {
    this._validators = {
        'success': function (data) { return data.success; },
        'enabled': function (data) { return data.is_enabled; }
    }
};

TwoStateToggle.prototype.setDataValidator = function (name, func) {
    if (name === 'success' || name === 'enabled') {
        this._validators[name] = func;
    } else {
        throw 'unknown validator name ' + name;
    }
};

/**
 * func must either return `true` or `false`
 * if `false` is returned, data submission will be canceled
 */
TwoStateToggle.prototype.setBeforeSubmitHandler = function(func) {
    this._beforeSubmitHandler = func;
};

TwoStateToggle.prototype.getBeforeSubmitHandler = function () {
    return this._beforeSubmitHandler;
};

TwoStateToggle.prototype.datumIsValid = function (validatorName, data) {
    return this._validators[validatorName](data);
};

TwoStateToggle.prototype.getDefaultHandler = function () {
    var me = this;
    return function () {
        var handler = me.getBeforeSubmitHandler();
        if (handler && handler() === false) {
            return;
        }
        var data = me.getPostData();
        data.disable = me.isOn();
        /* @todo: need ability to prevent the ajax call
         * and do something else in certain conditions.
         * For example - invite an unauthenticated user to log in.
         * This functionality can be better
         * defined in the "SimpleControl".
         */
        $.ajax({
            type: 'POST',
            dataType: 'json',
            cache: false,
            url: me.toggleUrl,
            data: data,
            success: function (data) {
                if (me.datumIsValid('success', data)) {
                    if (me.datumIsValid('enabled', data)) {
                        me.setState('on-state');
                    } else {
                        me.setState('off-state');
                    }
                    me.getElement().trigger('askbot.two-state-toggle.success', data);
                } else {
                    if (data.message) {
                        showMessage(me.getElement(), data.message);
                    }
                    me.getElement().trigger('askbot.two-state-toggle.error', data);
                }
            }
        });
    };
};

TwoStateToggle.prototype.isCheckBox = function () {
    var element = this._element;
    return element.attr('type') === 'checkbox';
};

TwoStateToggle.prototype.setState = function (state) {
    var element = this._element;
    this._state = state;
    if (element) {
        this.resetStyles();
        element.addClass(state);
        if (state === 'on-state') {
            element.data('isOn', true);
        } else if (state === 'off-state') {
            element.data('isOn', false);
        }
        if (this.isCheckBox()) {
            if (state === 'on-state') {
                element.attr('checked', true);
            } else if (state === 'off-state') {
                element.attr('checked', false);
            }
        } else {
            this.setText(this._messages[state]);
        }
    }
};

TwoStateToggle.prototype.setText = function (text) {
    var btnText = this._element.find('.js-btn-text');
    var where  = btnText.length ? btnText : this._element;
    where.html(text);
};

TwoStateToggle.prototype.decorate = function (element) {
    this._element = element;
    //read messages for all states
    var messages = {};
    messages['on-state'] = element.data('onStateText') || gettext('enabled');
    messages['off-state'] = element.data('offStateText') || gettext('disabled');
    messages['on-prompt'] = element.data('onPromptText') || messages['on-state'];
    messages['off-prompt'] = element.data('offPromptText') || messages['off-state'];
    this._messages = messages;

    this.toggleUrl = element.data('toggleUrl');

    //detect state and save it
    if (this.isCheckBox()) {
        this._state = element.is(':checked') ? 'on-state' : 'off-state';
    } else {
        this._state = element.data('isOn') ? 'on-state' : 'off-state';
    }

    //set mouseover handler only for non-checkbox version
    if (this.isCheckBox() === false) {
        var me = this;
        element.mouseover(function () {
            var is_on = me.isOn();
            if (is_on) {
                me.setState('off-prompt');
            } else {
                me.setState('on-prompt');
            }
            return false;
        });
        element.mouseout(function () {
            var is_on = me.isOn();
            if (is_on) {
                me.setState('on-state');
            } else {
                me.setState('off-state');
            }
            return false;
        });
    }

    setupButtonEventHandlers(element, this.getHandler());
};
