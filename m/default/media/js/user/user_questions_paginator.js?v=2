var UserQuestionsPaginator = function () {
    Paginator.call(this);
};
inherits(UserQuestionsPaginator, Paginator);

UserQuestionsPaginator.prototype.renderPage = function (data) {
    $('.users-questions').html(data.questions);
    $('.timeago').timeago();
};

UserQuestionsPaginator.prototype.getPageDataUrl = function (pageNo) {
    var userId = askbot.data.viewUserId;
    var pageSize = askbot.data.userPostsPageSize;
    var url = QSutils.patch_query_string('', 'author:' + userId);
    url = QSutils.patch_query_string(url, 'sort:votes-desc');
    url = QSutils.patch_query_string(url, 'page:' + pageNo);
    url = QSutils.patch_query_string(url, 'page-size:' + pageSize);
    return askbot.urls.questions + url;
};
