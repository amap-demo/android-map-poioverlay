# android-map-poioverlay
PoiOverlay开源代码及调用

本工程为基于高德地图Android SDK进行封装，开源了PoiOverlay的代码。
## 前述 ##
- [高德官网申请Key](http://lbs.amap.com/dev/#/).
- 阅读[参考手册](http://a.amap.com/lbs/static/unzip/Android_Map_Doc/index.html).
- 工程基于3D地图SDK实现

##效果展示##
![Screenshot]( https://github.com/amap-demo/android-map-poioverlay/raw/master/apk/picture.png )

## 扫一扫安装##
![Screenshot]( https://github.com/amap-demo/android-map-poioverlay/raw/master/apk/1480320135.png )

## 使用方法##
###1:配置搭建AndroidSDK工程###
- [Android Studio工程搭建方法](http://lbs.amap.com/api/android-sdk/guide/creat-project/android-studio-creat-project/#add-jars).
- [通过maven库引入SDK方法](http://lbsbbs.amap.com/forum.php?mod=viewthread&tid=18786).

## 核心类/接口 ##
| 类    | 接口  | 说明   |
| -----|:-----:|:-----:|
| PoiOverlay | PoiOverlay(AMap amap, java.util.List<com.amap.api.services.core.PoiItem> pois) | 通过此构造函数创建Poi图层。 |
| PoiOverlay | 	addToMap() | 添加Marker到地图中。 |
| PoiOverlay | 	removeFromMap() | 去掉PoiOverlay上所有的Marker。 |
| PoiOverlay | 	zoomToSpan() | 移动镜头到当前的视角。 |
| PoiOverlay | 	addToMap() | 添加Marker到地图中。 |

## 核心难点 ##
```
/**
	 * 添加Marker到地图中。
	 */
	public void addToMap() {
		try{
			for (int i = 0; i < mPois.size(); i++) {
				Marker marker = mAMap.addMarker(getMarkerOptions(i));
				marker.setObject(i);
				mPoiMarks.add(marker);
			}
		}catch(Throwable e){
			e.printStackTrace();
		}
	}
	/**
	 * 去掉PoiOverlay上所有的Marker。
	 */
	public void removeFromMap() {
		for (Marker mark : mPoiMarks) {
			mark.remove();
		}
	}
	/**
	 * 移动镜头到当前的视角。
	 */
	public void zoomToSpan() {
		try{
			if (mPois != null && mPois.size() > 0) {
				if (mAMap == null)
					return;
				if(mPois.size()==1){
					mAMap.moveCamera(CameraUpdateFactory.newLatLngZoom(new LatLng(mPois.get(0).getLatLonPoint().getLatitude(),
							mPois.get(0).getLatLonPoint().getLongitude()), 18f));
				}else{
					LatLngBounds bounds = getLatLngBounds();
					mAMap.moveCamera(CameraUpdateFactory.newLatLngBounds(bounds, 30));
				}
			}
		}catch(Throwable e){
			e.printStackTrace();
		}
	}
//   获取Bounds
	private LatLngBounds getLatLngBounds() {
		LatLngBounds.Builder b = LatLngBounds.builder();
		for (int i = 0; i < mPois.size(); i++) {
			b.include(new LatLng(mPois.get(i).getLatLonPoint().getLatitude(),
					mPois.get(i).getLatLonPoint().getLongitude()));
		}
		return b.build();
	}
```
