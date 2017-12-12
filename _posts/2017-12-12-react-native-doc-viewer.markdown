---
title: "React Native 文档查看组件"
layout: post
date: 2017-12-05 14:33
image: /assets/images/markdown.jpg
headerImage: false
tag:
- react native
star: false
category: blog
author: forrest23
description: React Native 文档查看组件。

---
*本文原创首发于公众号：ReactNative开发圈，转载需注明出处。*

> React Native 文档查看组件：react-native-doc-viewer，可以在手机上直接打开文档，支持远程和本地文档。支持的文档格式：xls,ppt,doc,xlsx,pptx,docx,png,jpg,pdf,mp4。支持iOS和Android。  

## 安装方法
```
npm install react-native-doc-viewer --save
react-native link react-native-doc-viewer
```

## API说明
* openDoc  打开远程货或本地文档
* openDocb64 打开Base64字符串格式文档
* openDocBinaryinUrl  打开二进制文件
* playMovie  打开视频文件

![](http://pic.yupoo.com/forrest071/GXPRrG2B/AClbX.png)

## 使用示例
```
import React, { Component } from 'react';
import {
    AppRegistry,
    StyleSheet,
    Text,
    View,
    Platform,
    Button,
    Alert,
    ActivityIndicator
} from 'react-native';
import OpenFile from 'react-native-doc-viewer';
var RNFS = require('react-native-fs');
var SavePath = Platform.OS === 'ios' ? RNFS.MainBundlePath : RNFS.DocumentDirectoryPath;
export default class Component02 extends Component {

    static navigationOptions = ({ navigation }) => ({
        title: `${navigation.state.params.name}`,
    });

    state = { animating: false }
    /*
    * Handle WWW File Method
    * fileType Default == "" you can use it, to set the File Extension (pdf,doc,xls,ppt etc) when in the Url the File Extension is missing.
    */
    handlePress = () => {
        this.setState({ animating: true });
        if (Platform.OS === 'ios') {
            OpenFile.openDoc([{
                url: "https://calibre-ebook.com/downloads/demos/demo.docx",
                fileNameOptional: "test filename"
            }], (error, url) => {
                if (error) {
                    this.setState({ animating: false });
                } else {
                    this.setState({ animating: false });
                    console.log(url)
                }
            })
        } else {
            //Android
            this.setState({ animating: true });
            OpenFile.openDoc([{
                url: "https://www.huf-haus.com/fileadmin/Bilder/Header/ART_3/Header_HUF_Haus_ART_3___1_.jpg", // Local "file://" + filepath
                fileName: "sample",
                cache: false,
                fileType: "jpg"
            }], (error, url) => {
                if (error) {
                    this.setState({ animating: false });
                    console.error(error);
                } else {
                    this.setState({ animating: false });
                    console.log(url)
                }
            })
        }

    }


    /*
    * Handle Local File Method
    * fileType Default == "" you can use it, to set the File Extension (pdf,doc,xls,ppt etc) when in the Url the File Extension is missing.
    */
    handlePressLocal = () => {
        this.setState({ animating: true });
        if (Platform.OS === 'ios') {
            OpenFile.openDoc([{
                url: SavePath + "demo.docx",
                fileNameOptional: "test filename"
            }], (error, url) => {
                if (error) {
                    this.setState({ animating: false });
                } else {
                    this.setState({ animating: false });
                    console.log(url)
                }
            })
        } else {
            OpenFile.openDoc([{
                url: SavePath + "demo.jpg",
                fileName: "sample",
                cache: false,
                fileType: "jpg"
            }], (error, url) => {
                if (error) {
                    this.setState({ animating: false });
                } else {
                    this.setState({ animating: false });
                    console.log(url)
                }
            })

        }

    }

    /*
    * Binary in URL
    * Binary String in Url
    * fileType Default == "" you can use it, to set the File Extension (pdf,doc,xls,ppt etc) when in the Url you dont have an File Extensions
    */
    handlePressBinaryinUrl = () => {
        this.setState({ animating: true });
        if (Platform.OS === 'ios') {
            OpenFile.openDocBinaryinUrl([{
                url: "https://storage.googleapis.com/need-sure/example",
                fileName: "sample",
                fileType: "xml"
            }], (error, url) => {
                if (error) {
                    console.error(error);
                    this.setState({ animating: false });
                } else {
                    console.log(url)
                    this.setState({ animating: false });
                }
            })
        } else {
            OpenFile.openDocBinaryinUrl([{
                url: "https://storage.googleapis.com/need-sure/example",
                fileName: "sample",
                fileType: "xml",
                cache: true
            }], (error, url) => {
                if (error) {
                    console.error(error);
                    this.setState({ animating: false });
                } else {
                    console.log(url)
                    this.setState({ animating: false });
                }
            })
        }
    }
    /*
    * Base64String
    * put only the base64 String without data:application/octet-stream;base64
    * fileType Default == "" you can use it, to set the File Extension (pdf,doc,xls,ppt etc) when in the Url you dont have an File Extensions
    */
    handlePressb64 = () => {
        this.setState({ animating: true });
        if (Platform.OS === 'ios') {
            OpenFile.openDocb64([{
                base64: "",
                fileName: "sample.png",
                fileType: "png"
            }], (error, url) => {
                if (error) {
                    console.error(error);
                    this.setState({ animating: false });
                } else {
                    console.log(url)
                    this.setState({ animating: false });
                }
            })
        } else {
            OpenFile.openDocb64([{
                base64: "",
                fileName: "sample",
                fileType: "png",
                cache: true
            }], (error, url) => {
                if (error) {
                    console.error(error);
                    this.setState({ animating: false });
                } else {
                    console.log(url)
                    this.setState({ animating: false });
                }
            })
        }
    }

    /*
    * mp4 Video
    */
    handlePressVideo(video) {
        this.setState({ animating: true });
        if (Platform.OS === 'ios') {
            OpenFile.playMovie(video, (error, url) => {
                if (error) {
                    console.error(error);
                    this.setState({ animating: false });
                } else {
                    console.log(url)
                    this.setState({ animating: false });
                }
            })
        } else {
            this.setState({ animating: false });
            Alert.alert("Coming soon for Android")
        }
    }

    setToggleTimeout() {
        this.setTimeout(() => {
            this.setState({ animating: !this.state.animating });
            this.setToggleTimeout();
        }, 2000);
    }

    render() {
        return (

            <View style={styles.container}>
                <ActivityIndicator
                    animating={this.state.animating}
                    style={[styles.centering, { height: 80 }]}
                    size="large" />
                <Text style={styles.welcome}>
                    Doc Viewer React Native
        </Text>
                <Button
                    onPress={this.handlePress.bind(this)}
                    title="打开远程文档"
                    accessibilityLabel="See a Document"
                />
                <Button
                    onPress={this.handlePressLocal.bind(this)}
                    title="打开本地文档"
                    accessibilityLabel="See a Document"
                />
                <Button
                    onPress={this.handlePressBinaryinUrl.bind(this)}
                    title="打开远程二进制文档"
                    accessibilityLabel="See a Document"
                />
                <Button
                    onPress={this.handlePressb64.bind(this)}
                    title="打开Base64字符串"
                    accessibilityLabel="See a Document"
                />
                <Button
                    onPress={() => this.handlePressVideo("http://www.sample-videos.com/video/mp4/720/big_buck_bunny_720p_1mb.mp4")}
                    title="打开视频"
                    accessibilityLabel="See a Document"
                />
            </View>
        );
    }
}

const styles = StyleSheet.create({
    container: {
        flex: 1,
        justifyContent: 'center',
        alignItems: 'center',
        backgroundColor: '#F5FCFF',
    },
    welcome: {
        fontSize: 20,
        textAlign: 'center',
        margin: 10,
    },
    instructions: {
        textAlign: 'center',
        color: '#333333',
        marginBottom: 5,
    },
});
```

## Screenshots
![](http://pic.yupoo.com/forrest071/GXPRrN4k/10QQAD.png)

## 示例源码
[GitHub - forrest23/ReactNativeComponents: React Native组件大全](https://github.com/forrest23/ReactNativeComponents)

## 组件地址
[GitHub - philipphecht/react-native-doc-viewer: React Native Doc Viewer (Supports file formats: xls,ppt,doc,xlsx,pptx,docx,png,jpg,pdf)](https://github.com/philipphecht/react-native-doc-viewer)


举手之劳关注我的微信公众号：**ReactNative开发圈**
![Screenshot](http://pic.yupoo.com/forrest071/GW9CBRAi/medium.jpg)
