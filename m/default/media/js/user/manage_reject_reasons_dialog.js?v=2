/**
 * @constructor
 * manages post/edit reject reasons
 * in the post moderation view
 */
var ManageRejectReasonsDialog = function () {
    WrappedElement.call(this);
    this._selected_edit_ids = null;
    this._selected_reason_id = null;
    this._state = null;//'select', 'add-new'
    this._postModerationControls = [];
    this._selectedEditDataReader = undefined;
};
inherits(ManageRejectReasonsDialog, WrappedElement);

ManageRejectReasonsDialog.prototype.setMenu = function (menu) {
    this._reasonsMenu = menu;
};

ManageRejectReasonsDialog.prototype.getMenu = function () {
    return this._reasonsMenu;
};

ManageRejectReasonsDialog.prototype.setSelectedEditDataReader = function (func) {
    this._selectedEditDataReader = func;
};

ManageRejectReasonsDialog.prototype.readSelectedEditData = function () {
    var data = this._selectedEditDataReader();
    this.setSelectedEditData(data);
    return data.id_list.length > 0;
};

ManageRejectReasonsDialog.prototype.setSelectedEditData = function (data) {
    this._selected_edit_data = data;
};

ManageRejectReasonsDialog.prototype.addPostModerationControl = function (control) {
    this._postModerationControls.push(control);
};

ManageRejectReasonsDialog.prototype.setState = function (state) {
    this._state = state;
    this.clearErrors();
    if (this._element) {
        this._selector.hide();
        this._adder.hide();
        if (state === 'select') {
            this._selector.show();
        } else if (state === 'add-new') {
            this._adder.show();
        }
    }
};

ManageRejectReasonsDialog.prototype.show = function () {
    $(this._element).modal('show');
};

ManageRejectReasonsDialog.prototype.hide = function () {
    $(this._element).modal('hide');
};

ManageRejectReasonsDialog.prototype.resetInputs = function () {
    if (this._title_input) {
        this._title_input.reset();
    }
    if (this._details_input) {
        this._details_input.reset();
    }
    var selected = this._element.find('.selected');
    selected.removeClass('selected');
};

ManageRejectReasonsDialog.prototype.clearErrors = function () {
    var error = this._element.find('.alert');
    error.remove();
};

ManageRejectReasonsDialog.prototype.makeAlertBox = function (errors) {
    //construct the alert box
    var alert_box = new AlertBox();
    alert_box.setClass('alert-error');
    if (typeof errors === 'string') {
        alert_box.setText(errors);
    } else if (errors.constructor === [].constructor) {
        if (errors.length > 1) {
            alert_box.setContent(
                '<div>' +
                gettext('Looks there are some things to fix:') +
                '</div>'
            );
            var list = this.makeElement('ul');
            $.each(errors, function (idx, item) {
                list.append('<li>' + item + '</li>');
            });
            alert_box.addContent(list);
        } else if (errors.length === 1) {
            alert_box.setContent(errors[0]);
        } else if (errors.length === 0) {
            return;
        }
    } else if ('html' in errors) {
        alert_box.setContent(errors);
    } else {
        return;//don't know what to do
    }
    return alert_box;
};

ManageRejectReasonsDialog.prototype.setAdderErrors = function (errors) {
    //clear previous errors
    this.clearErrors();
    var alert_box = this.makeAlertBox(errors);
    this._element
        .find('#reject-edit-modal-add-new .modal-body')
        .prepend(alert_box.getElement());
};

ManageRejectReasonsDialog.prototype.setSelectorErrors = function (errors) {
    this.clearErrors();
    var alert_box = this.makeAlertBox(errors);
    this._element
        .find('#reject-edit-modal-select .modal-body')
        .prepend(alert_box.getElement());
};

ManageRejectReasonsDialog.prototype.setErrors = function (errors) {
    this.clearErrors();
    var alert_box = this.makeAlertBox(errors);
    var current_state = this._state;
    this._element
        .find('#reject-edit-modal-' + current_state + ' .modal-body')
        .prepend(alert_box.getElement());
};

ManageRejectReasonsDialog.prototype.addSelectableReason = function (data) {
    var id = data.reason_id;
    var title = data.title;
    var details = data.details;
    this._select_box.addItem(id, title, details);

    askbot.data.postRejectReasons.push(
        {id: data.reason_id, title: data.title}
    );
    $.each(this._postModerationControls, function (idx, control) {
        control.addReason(data.reason_id, data.title);
    });
};

ManageRejectReasonsDialog.prototype.startSavingReason = function (callback) {

    var title_input = this._title_input;
    var details_input = this._details_input;

    var errors = [];
    if (title_input.isBlank()) {
        errors.push(gettext('Please provide description.'));
    }
    if (details_input.isBlank()) {
        errors.push(gettext('Please provide details.'));
    }

    if (errors.length > 0) {
        this.setAdderErrors(errors);
        return;//just show errors and quit
    }

    var data = {
        title: title_input.getVal(),
        details: details_input.getVal()
    };
    var reasonIsNew = true;
    if (this._selected_reason_id) {
        data.reason_id = this._selected_reason_id;
        reasonIsNew = false;
    }

    var me = this;

    $.ajax({
        type: 'POST',
        dataType: 'json',
        cache: false,
        url: askbot.urls.save_post_reject_reason,
        data: data,
        success: function (data) {
            if (data.success) {
                //show current reason data and focus on it
                me.addSelectableReason(data);
                if (reasonIsNew) {
                    me.getMenu().addReason(data.reason_id, data.title);
                }
                if (callback) {
                    callback(data);
                } else {
                    me.setState('select');
                }
            } else {
                me.setAdderErrors(data.message);
            }
        }
    });
};

ManageRejectReasonsDialog.prototype.startEditingReason = function () {
    var data = this._select_box.getSelectedItemData();
    var title = $(data.title).text();
    var details = data.details;
    this._title_input.setVal(title);
    this._details_input.setVal(details);
    this._selected_reason_id = data.id;
    this.setState('add-new');
};

ManageRejectReasonsDialog.prototype.resetSelectedReasonId = function () {
    this._selected_reason_id = null;
};

ManageRejectReasonsDialog.prototype.getSelectedReasonId = function () {
    return this._selected_reason_id;
};

ManageRejectReasonsDialog.prototype.startDeletingReason = function () {
    var select_box = this._select_box;
    var data = select_box.getSelectedItemData();
    var reason_id = data.id;
    var me = this;
    if (data.id) {
        $.ajax({
            type: 'POST',
            dataType: 'json',
            cache: false,
            url: askbot.urls.delete_post_reject_reason,
            data: {reason_id: reason_id},
            success: function (data) {
                if (data.success) {
                    select_box.removeItem(reason_id);
                    me.hideEditButtons();
                    me.getMenu().removeReason(reason_id);
                } else {
                    me.setSelectorErrors(data.message);
                }
            }
        });
    } else {
        me.setSelectorErrors(
            gettext('A reason must be selected to delete one.')
        );
    }
};

ManageRejectReasonsDialog.prototype.hideEditButtons = function () {
    this._editButton.hide();
    this._deleteButton.hide();
};

ManageRejectReasonsDialog.prototype.showEditButtons = function () {
    this._editButton.show();
    this._deleteButton.show();
};

ManageRejectReasonsDialog.prototype.decorate = function (element) {
    this._element = element;
    //set default state according to the # of available reasons
    this._selector = $(element).find('#reject-edit-modal-select');
    this._adder = $(element).find('#reject-edit-modal-add-new');
    if (this._selector.find('li').length > 0) {
        this.setState('select');
        this.resetInputs();
    } else {
        this.setState('add-new');
        this.resetInputs();
    }

    var select_box = new SelectBox();
    select_box.decorate($(this._selector.find('.select-box')));
    select_box.setSelectHandler(function () { me.showEditButtons(); });
    this._select_box = select_box;

    //setup tipped-inputs
    var reject_title_input = $(this._element).find('input');
    var title_input = new TippedInput();
    title_input.decorate($(reject_title_input));
    this._title_input = title_input;

    var reject_details_input = $(this._element).find('textarea.reject-reason-details');

    var details_input = new TippedInput();
    details_input.decorate($(reject_details_input));
    this._details_input = details_input;

    var me = this;
    setupButtonEventHandlers(
        element.find('.cancel, .modal-header .close'),
        function () {
            me.hide();
            me.clearErrors();
            me.resetInputs();
            me.resetSelectedReasonId();
            me.setState('select');
            me.hideEditButtons();
        }
    );

    setupButtonEventHandlers(
        $(this._element).find('.save-reason'),
        function () { me.startSavingReason(); }
    );

    setupButtonEventHandlers(
        element.find('.add-new-reason'),
        function () {
            me.resetSelectedReasonId();
            me.resetInputs();
            me.setState('add-new') ;
        }
    );

    this._editButton = element.find('.edit-this-reason');
    setupButtonEventHandlers(
        this._editButton,
        function () {
            me.startEditingReason();
        }
    );

    this._deleteButton = element.find('.delete-this-reason');
    setupButtonEventHandlers(
        this._deleteButton,
        function () {
            me.startDeletingReason();
        }
    );
};
