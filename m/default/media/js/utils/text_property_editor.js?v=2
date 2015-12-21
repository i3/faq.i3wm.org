/**
 * attaches a modal menu with a text editor
 * to a link. The modal menu is from bootstrap.js
 * todo: this should probably be a subclass of ModalDialog,
 * triggered by a link click, then a whole bunch of methods
 * would be simply inherited from the modal dialog:
 * clearMessages, etc.
 */
var TextPropertyEditor = function () {
    WrappedElement.call(this);
    this._editor = null;
};
inherits(TextPropertyEditor, WrappedElement);

TextPropertyEditor.prototype.getWidgetData = function () {
    var data = this._element.data();
    return {
        object_id: data.objectId,
        model_name: data.modelName,
        property_name: data.propertyName,
        url: data.url,
        help_text: data.helpText,
        editor_heading: data.editorHeading
    };
};

TextPropertyEditor.prototype.makeEditor = function () {
    if (this._editor) {
        return this._editor;
    }
    var editor = new ModalDialog();
    this._editor = editor;
    editor.setHeadingText(this.getWidgetData().editor_heading);

    //create main content for the editor
    var textarea = this.makeElement('textarea');
    textarea.addClass('tipped-input blank');
    textarea.val(this.getWidgetData().help_text);

    var tipped_input = new TippedInput();
    tipped_input.decorate(textarea);
    this._text_input = tipped_input;

    editor.setContent(textarea);
    //body.append(textarea);

    editor.setAcceptButtonText(gettext('Save'));
    editor.setRejectButtonText(gettext('Cancel'));

    var me = this;
    editor.setAcceptHandler(function () {
        me.saveData();
    });

    $('body').append(editor.getElement());
    return editor;
};

TextPropertyEditor.prototype.openEditor = function () {
    this._editor.show();
};

TextPropertyEditor.prototype.clearMessages = function () {
    this._editor.clearMessages();
};

TextPropertyEditor.prototype.showAlert = function (text) {
    this._editor.setMessage(text, 'alert');
};

TextPropertyEditor.prototype.showError = function (text) {
    this._editor.setMessage(text, 'error');
};

TextPropertyEditor.prototype.setText = function (text) {
    this._text_input.setVal(text);
};

TextPropertyEditor.prototype.getText = function () {
    return this._text_input.getVal();
};

TextPropertyEditor.prototype.hideDialog = function () {
    this._editor.hide();
};

TextPropertyEditor.prototype.startOpeningEditor = function () {
    var me = this;
    $.ajax({
        type: 'GET',
        dataType: 'json',
        cache: false,
        url: me.getWidgetData().url,
        data: me.getWidgetData(),
        success: function (data) {
            if (data.success) {
                me.makeEditor();
                me.setText($.trim(data.text));
                me.openEditor();
            } else {
                showMessage(me.getElement(), data.message);
            }
        }
    });
};

TextPropertyEditor.prototype.saveData = function () {
    var data = this.getWidgetData();
    data.text = this.getText();
    var me = this;
    $.ajax({
        type: 'POST',
        dataType: 'json',
        cache: false,
        url: me.getWidgetData().url,
        data: data,
        success: function (data) {
            if (data.success) {
                me.showAlert(gettext('saved'));
                setTimeout(function () {
                    me.clearMessages();
                    me.hideDialog();
                }, 1000);
            } else {
                me.showError(data.message);
            }
        }
    });
};

TextPropertyEditor.prototype.decorate = function (element) {
    this._element = element;
    var me = this;
    setupButtonEventHandlers(element, function () { me.startOpeningEditor(); });
};
