/**
* the dropdown menu with selection of reasons
* to reject posts and a button that starts menu to
* manage the list of reasons
*/
var DeclineAndExplainMenu = function () {
    WrappedElement.call(this);
};
inherits(DeclineAndExplainMenu, WrappedElement);

DeclineAndExplainMenu.prototype.setupDeclinePostHandler = function (button) {
    var me = this;
    var reasonId = button.data('reasonId');
    var controls = this.getControls();
    var handler = controls.getModHandler('decline-with-reason', ['posts'], reasonId);
    setupButtonEventHandlers(button, handler);
};

DeclineAndExplainMenu.prototype.addReason = function (id, title) {
    var li = this.makeElement('li');
    var button = this.makeElement('a');
    li.append(button);
    button.html(title);
    button.data('reasonId', id);
    button.attr('data-reason-id', id);
    this._addReasonBtn.parent().before(li);

    this.setupDeclinePostHandler(button);
};

DeclineAndExplainMenu.prototype.removeReason = function (id) {
    var btn = this._element.find('a[data-reason-id="' + id + '"]');
    btn.parent().remove();
};

DeclineAndExplainMenu.prototype.setControls = function (controls) {
    this._controls = controls;
};

DeclineAndExplainMenu.prototype.getControls = function () {
    return this._controls;
};

DeclineAndExplainMenu.prototype.decorate = function (element) {
    this._element = element;
    //activate dropdown menu
    element.dropdown();

    var declineBtns = element.find('.decline-with-reason');
    var me = this;
    declineBtns.each(function (idx, elem) {
        me.setupDeclinePostHandler($(elem));
    });

    this._reasonList = element.find('ul');

    var addReasonBtn = element.find('.manage-reasons');
    this._addReasonBtn = addReasonBtn;

    var manageReasonsDialog = new ManageRejectReasonsDialog();
    manageReasonsDialog.decorate($('#manage-reject-reasons-modal'));
    this._manageReasonsDialog = manageReasonsDialog;
    manageReasonsDialog.setMenu(this);

    setupButtonEventHandlers(addReasonBtn, function () { manageReasonsDialog.show(); });
};
