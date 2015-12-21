
var UserAnswersPaginator = function () {
    Paginator.call(this);
};
inherits(UserAnswersPaginator, Paginator);

UserAnswersPaginator.prototype.renderPage = function (data) {
    $('.users-answers').html(data.html);
    $('.timeago').timeago();
};

UserAnswersPaginator.prototype.getPageDataUrl = function () {
    return askbot.urls.getTopAnswers;
};

UserAnswersPaginator.prototype.getPageDataUrlParams = function (pageNo) {
    return {
        user_id: askbot.data.viewUserId,
        page_number: pageNo
    };
};
