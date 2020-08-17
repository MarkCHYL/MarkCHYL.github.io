---
layout: post
title: 环信easeUI的集成
date: 2018-12-19 16:27:09
toc: true
reward: true
tags: [Android,第三方集成]
---
关于环信easeUI的集成
环信的集成，网上有许多的demo，看的每个人有点眼花缭乱，对于官方的文档的吐槽也有许多，确实在集成第三方的那些玩意时，不管怎样我们必须先得看看她们的官方介绍。前段时我遇到的项目需要集成环信的聊天功能，一个会话列表和聊天界面，网上搜了很多，由于版本不一样后来还是选择结合官方的demo和开发文档写了个demo。
我们建议先看一下环信的官方文档，地址：
点击打开链接
<!--more-->
至于基础的集成和导入SDK我在这就不造次啦！环信官方上的文档介绍的很好，看不懂的可以看上面的视频介绍；
下面我直接说说我的步骤，后面也会贴上源码的地址，望大家不嫌弃，喜欢就给个 start


一、首先是初始化环信easeui：


  /**
     * 初始化环信SDK
     */
    private void initEasemob() {

        if (EaseUI.getInstance().init(mContext, initOptions())) {

            // 设置开启debug模式
            EMClient.getInstance().setDebugMode(true);

            // 设置初始化已经完成
            isInit = true;
        }
    }

关于sdk的一些配置初始化，请看demo源码





二、登陆和注册的：

登陆：



 /**
     * 登录方法
     */
    private void signIn() {
        final String username = mUsernameEdit.getText().toString().trim();
        final String password = mPasswordEdit.getText().toString().trim();
        if (TextUtils.isEmpty(username) || TextUtils.isEmpty(password)) {
            Toast.makeText(LoginRegisterActivity.this, "用户名和密码不能为空", Toast.LENGTH_LONG).show();
            return;
        }
        EMClient.getInstance().login(username, password, new EMCallBack() {
            /**
             * 登陆成功的回调
             */
            @Override
            public void onSuccess() {
                mDialog.dismiss();
                Message msg=new Message();
                msg.what=2;
                mHandler.sendMessage(msg);
                // 加载所有会话到内存
                EMClient.getInstance().chatManager().loadAllConversations();
                // 登录成功跳转界面
                Intent intent = new Intent(LoginRegisterActivity.this, MainActivity.class);
                intent.putExtra("userName",username);
                startActivity(intent);
                finish();
            }

            /**
             * 登陆错误的回调
             * @param i
             * @param s
             */
            @Override
            public void onError(final int i, final String s) {
                runOnUiThread(new Runnable() {
                    @Override
                    public void run() {
                        mDialog.dismiss();
                        Log.d("lzan13", "登录失败 Error code:" + i + ", message:" + s);
                        /**
                         * 关于错误码可以参考官方api详细说明
                         * http://www.easemob.com/apidoc/android/chat3.0/classcom_1_1hyphenate_1_1_e_m_error.html
                         */
                        switch (i) {
                        // 网络异常 2
                        case EMError.NETWORK_ERROR:
                            Toast.makeText(LoginRegisterActivity.this, "网络错误 code: " + i + ", message:" + s, Toast.LENGTH_LONG).show();
                            break;
                        // 无效的用户名 101
                        case EMError.INVALID_USER_NAME:
                            Toast.makeText(LoginRegisterActivity.this, "无效的用户名 code: " + i + ", message:" + s, Toast.LENGTH_LONG).show();
                            break;
                        // 无效的密码 102
                        case EMError.INVALID_PASSWORD:
                            Toast.makeText(LoginRegisterActivity.this, "无效的密码 code: " + i + ", message:" + s, Toast.LENGTH_LONG).show();
                            break;
                        // 用户认证失败，用户名或密码错误 202
                        case EMError.USER_AUTHENTICATION_FAILED:
                            Toast.makeText(LoginRegisterActivity.this, "用户认证失败，用户名或密码错误 code: " + i + ", message:" + s, Toast.LENGTH_LONG).show();
                            break;
                        // 用户不存在 204
                        case EMError.USER_NOT_FOUND:
                            Toast.makeText(LoginRegisterActivity.this, "用户不存在 code: " + i + ", message:" + s, Toast.LENGTH_LONG).show();
                            break;
                        // 无法访问到服务器 300
                        case EMError.SERVER_NOT_REACHABLE:
                            Toast.makeText(LoginRegisterActivity.this, "无法访问到服务器 code: " + i + ", message:" + s, Toast.LENGTH_LONG).show();
                            break;
                        // 等待服务器响应超时 301
                        case EMError.SERVER_TIMEOUT:
                            Toast.makeText(LoginRegisterActivity.this, "等待服务器响应超时 code: " + i + ", message:" + s, Toast.LENGTH_LONG).show();
                            break;
                        // 服务器繁忙 302
                        case EMError.SERVER_BUSY:
                            Toast.makeText(LoginRegisterActivity.this, "服务器繁忙 code: " + i + ", message:" + s, Toast.LENGTH_LONG).show();
                            break;
                        // 未知 Server 异常 303 一般断网会出现这个错误
                        case EMError.SERVER_UNKNOWN_ERROR:
                            Toast.makeText(LoginRegisterActivity.this, "未知的服务器异常 code: " + i + ", message:" + s, Toast.LENGTH_LONG).show();
                            break;
                        default:
                            Toast.makeText(LoginRegisterActivity.this, "ml_sign_in_failed code: " + i + ", message:" + s, Toast.LENGTH_LONG).show();
                            break;
                        }
                    }
                });
            }

            @Override
            public void onProgress(int i, String s) {
                mDialog.show(LoginRegisterActivity.this,"登录", "正在登录中");
            }
        });
    }







注册：



private void signUp() {
    // 注册是耗时过程，所以要显示一个dialog来提示下用户
    mDialog.setMessage("注册中，请稍后...");
    mDialog.show();
    new Thread(new Runnable() {
        @Override
        public void run() {
            try {
                String username = mUsernameEdit.getText().toString().trim();
                String password = mPasswordEdit.getText().toString().trim();
                EMClient.getInstance().createAccount(username, password);
                runOnUiThread(new Runnable() {
                    @Override
                    public void run() {
                        if (!LoginRegisterActivity.this.isFinishing()) {
                            mDialog.dismiss();
                        }
                        Toast.makeText(LoginRegisterActivity.this, "注册成功", Toast.LENGTH_LONG).show();
                    }
                });
            } catch (final HyphenateException e) {
                e.printStackTrace();
                runOnUiThread(new Runnable() {
                    @Override
                    public void run() {
                        if (!LoginRegisterActivity.this.isFinishing()) {
                            mDialog.dismiss();
                        }
                        /**
                         * 关于错误码可以参考官方api详细说明
                         * http://www.easemob.com/apidoc/android/chat3.0/classcom_1_1hyphenate_1_1_e_m_error.html
                         */
                        int errorCode = e.getErrorCode();
                        String message = e.getMessage();
                        Log.d("lzan13", String.format("sign up - errorCode:%d, errorMsg:%s", errorCode, e.getMessage()));
                        switch (errorCode) {
                        // 网络错误
                        case EMError.NETWORK_ERROR:
                            Toast.makeText(LoginRegisterActivity.this, "网络错误 code: " + errorCode + ", message:" + message, Toast.LENGTH_LONG).show();
                            break;
                        // 用户已存在
                        case EMError.USER_ALREADY_EXIST:
                            Toast.makeText(LoginRegisterActivity.this, "用户已存在 code: " + errorCode + ", message:" + message, Toast.LENGTH_LONG).show();
                            break;
                        // 参数不合法，一般情况是username 使用了uuid导致，不能使用uuid注册
                        case EMError.USER_ILLEGAL_ARGUMENT:
                            Toast.makeText(LoginRegisterActivity.this, "参数不合法，一般情况是username 使用了uuid导致，不能使用uuid注册 code: " + errorCode + ", message:" + message, Toast.LENGTH_LONG).show();
                            break;
                        // 服务器未知错误
                        case EMError.SERVER_UNKNOWN_ERROR:
                            Toast.makeText(LoginRegisterActivity.this, "服务器未知错误 code: " + errorCode + ", message:" + message, Toast.LENGTH_LONG).show();
                            break;
                        case EMError.USER_REG_FAILED:
                            Toast.makeText(LoginRegisterActivity.this, "账户注册失败 code: " + errorCode + ", message:" + message, Toast.LENGTH_LONG).show();
                            break;
                        default:
                            Toast.makeText(LoginRegisterActivity.this, "ml_sign_up_failed code: " + errorCode + ", message:" + message, Toast.LENGTH_LONG).show();
                            break;
                        }
                    }
                });
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
    }).start();
}

三、ChatActivity聊天界面。 我用的原生





   // 这里直接使用EaseUI封装好的聊天界面
        chatFragment = new ChatFragment();
        // 将参数传递给聊天界面
        chatFragment.setArguments(getIntent().getExtras());
        getSupportFragmentManager().beginTransaction().add(R.id.layout_chat, chatFragment).commit();

四、会话列表






   //直接用环信的会话列表
        conversationListFragment = new EaseConversationListFragment();
        conversationListFragment.setArguments(getIntent().getExtras());
        conversationListFragment.setConversationListItemClickListener(
                new EaseConversationListFragment.EaseConversationListItemClickListener() {
                    @Override
                    public void onListItemClicked(EMConversation conversation) {
                        startActivity(new Intent(ConversationListActivity.this, ChatActivity.class)
                                .putExtra(EaseConstant.EXTRA_USER_ID, conversation.conversationId()));
                    }

                });
        getSupportFragmentManager().beginTransaction().add(R.id.ec_layout_list, conversationListFragment).commit();

第一次写博客 ，还望兄弟们没嫌弃，我在源码中的注释很详细
Demo源码链接
