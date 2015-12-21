/**
 * @contstructor
 * a simple dropdown select element
 * which saves data to the server on change
 */
var DropdownSelect = function () {
    WrappedElement.call(this);
};
inherits(DropdownSelect, WrappedElement);

DropdownSelect.prototype.setPostData = function (data) {
    this._postData = data;
};

/**
 * posts value of selection to the url given
 * with data-url and parameter called "value"
 */
DropdownSelect.prototype.saveChoice = function () {
    var element = this._element;
    var url = this._url;
    var data = this._postData;
    data.value = element.val();
    $.ajax({
        type: 'POST',
        dataType: 'json',
        data: data,
        cache: false,
        url: url,
        success: function (data) {
            if (!data.success) {
                showMessage(element, data.message);
            }
        }
    });
};

DropdownSelect.prototype.decorate = function (element) {
    this._element = element;
    this._url = $(element).data('url');
    var me = this;
    this._element.change(function () {
        me.saveChoice();
    });
};
