DTDC Tracking API - Java
================================
Use Java to track DTDC shipments with DTDC Tracking API.

Features
--------
- Real-time DTDC tracking.
- Batch DTDC tracking.
- Other features to manage your DTDC tracking.

Installation
------------

**Maven**

    $ <dependency>
    $ <groupId>io.github.trackingmores</groupId>
    $ <artifactId>trackingmore-sdk-java</artifactId>
    $ <version>1.0.3</version>
    $ </dependency>

**Gradle**

    $ implementation "io.github.trackingmores:trackingmore-sdk-java:1.0.3"

Quick Start
----------
Get the API key:

To use this API, you need to generate your API key.

- <a href="https://admin.trackingmore.com/developer/apikey" target="_blank" rel="noreferrer">
  Click here</a> to access TrackingMore admin.

- Go to the "Developer" section.

- Click "Generate API Key".

- Give a name to your API key, and click "Save" .


Then, start to track your DTDC shipments.

Usage
----------

Create a tracking (Real-time tracking):

    package com.trackingmore.example.dtdc;
    
    import com.trackingmore.TrackingMore;
    import com.trackingmore.exception.TrackingMoreException;
    import com.trackingmore.model.TrackingMoreResponse;
    
    import java.io.IOException;
    import java.util.List;
    
    public class CreateDTDCTrackingExample {
    
        public static void main(String[] args) {
            try {
                String apiKey = "your api key";
                TrackingMore trackingMore = new TrackingMore(apiKey);
                
                CreateTrackingParams createTrackingParams = new CreateTrackingParams();
                createTrackingParams.setTrackingNumber("D87726913");
                createTrackingParams.setCourierCode("dtdc");
                TrackingMoreResponse<Tracking> result = trackingMore.trackings.CreateTracking(createTrackingParams);
                System.out.println(result.getMeta().getCode());
                if(result.getData() != null){
                   Tracking trackings = result.getData();
                   System.out.println(trackings);
                   System.out.println(trackings.getTrackingNumber());
                }
            } catch (TrackingMoreException e) {
                System.err.println("error：" + e.getMessage());
            } catch (IOException e) {
                System.err.println("error：" + e.getMessage());
            }
        }
    
    }


Create trackings (Max. 40 tracking numbers create in one call):

    package com.trackingmore.example.dtdc;
    
    import com.trackingmore.TrackingMore;
    import com.trackingmore.exception.TrackingMoreException;
    import com.trackingmore.model.TrackingMoreResponse;
    
    import java.io.IOException;
    import java.util.List;
    
    public class CreateDTDCTrackingsExample {
    
        public static void main(String[] args) {
            try {
                String apiKey = "your api key";
                TrackingMore trackingMore = new TrackingMore(apiKey);
                
                List<CreateTrackingParams> paramsList = new ArrayList<>();
                
                CreateTrackingParams createTrackingParams1 = new CreateTrackingParams();
                createTrackingParams1.setTrackingNumber("D89505076");
                createTrackingParams1.setCourierCode("dtdc");
                
                CreateTrackingParams createTrackingParams2 = new CreateTrackingParams();
                createTrackingParams2.setTrackingNumber("D88762174");
                createTrackingParams2.setCourierCode("dtdc");
                
                paramsList.add(createTrackingParams1);
                paramsList.add(createTrackingParams2);
                
                TrackingMoreResponse<BatchResults> result = trackingMore.trackings.BatchCreateTrackings(paramsList);
                System.out.println(result.getMeta().getCode());
                BatchResults batchResults = result.getData();
                if(result.getData() != null){
                    System.out.println(batchResults);
                    for (BatchItem batchItem : batchResults.getSuccess()) {
                        String trackingNumber = batchItem.getTrackingNumber();
                        String courierCode = batchItem.getCourierCode();
                        System.out.println("Tracking Number: " + trackingNumber);
                        System.out.println("Courier Code: " + courierCode);
                    }
                    for (BatchItem batchItem : batchResults.getError()) {
                        String trackingNumber = batchItem.getTrackingNumber();
                        String courierCode = batchItem.getCourierCode();
                        System.out.println("Tracking Number: " + trackingNumber);
                        System.out.println("Courier Code: " + courierCode);
                    }
                }
            } catch (TrackingMoreException e) {
                System.err.println("error：" + e.getMessage());
            } catch (IOException e) {
                System.err.println("error：" + e.getMessage());
            }
        }
    }


Get status of the shipment:

    package com.trackingmore.example.dtdc;
    
    import com.trackingmore.TrackingMore;
    import com.trackingmore.exception.TrackingMoreException;
    import com.trackingmore.model.TrackingMoreResponse;
    
    import java.io.IOException;
    import java.util.List;
    
    public class GetDTDCTrackingExample {
    
        public static void main(String[] args) {
            try {
                String apiKey = "your api key";
                TrackingMore trackingMore = new TrackingMore(apiKey);
                
                GetTrackingResultsParams trackingParams = new GetTrackingResultsParams();
                # Perform queries based on various conditions
                # trackingParams.setTrackingNumbers("D89505076,D88762174");
                trackingParams.setCourierCode("dtdc");
                trackingParams.setCreatedDateMin("2023-08-23T06:00:00+00:00");
                trackingParams.setCreatedDateMax("2023-09-05T07:20:42+00:00");
                TrackingMoreResponse<List<Tracking>> result = trackingMore.trackings.GetTrackingResults(trackingParams);
                System.out.println(result.getMeta().getCode());
                List<Tracking> trackings = result.getData();
                for (Tracking tracking : trackings) {
                    String trackingNumber = tracking.getTrackingNumber();
                    String courierCode = tracking.getCourierCode();
                    
                    System.out.println("Tracking Number: " + trackingNumber);
                    System.out.println("Courier Code: " + courierCode);
                }
            } catch (TrackingMoreException e) {
                System.err.println("error：" + e.getMessage());
            } catch (IOException e) {
                System.err.println("error：" + e.getMessage());
            }
        }
    }


Update a tracking by ID:

    package com.trackingmore.example.dtdc;
    
    import com.trackingmore.TrackingMore;
    import com.trackingmore.exception.TrackingMoreException;
    import com.trackingmore.model.TrackingMoreResponse;
    
    import java.io.IOException;
    import java.util.List;
    
    public class UpdateDTDCTrackingExample {
    
        public static void main(String[] args) {
            try {
                String apiKey = "your api key";
                TrackingMore trackingMore = new TrackingMore(apiKey);
                
                String idString = "98d159578c3936598d7f202340081330";
                UpdateTrackingParams updateTrackingParams = new UpdateTrackingParams();
                updateTrackingParams.setCustomerName("New name");
                updateTrackingParams.setNote("New tests order note");
                TrackingMoreResponse<UpdateTracking> result = trackingMore.trackings.UpdateTrackingByID(idString, updateTrackingParams);
                System.out.println(result.getMeta().getCode());
                System.out.println(result.getData());
                if(result.getData() != null){
                    UpdateTracking  updateTracking= result.getData();
                    System.out.println(updateTracking);
                    System.out.println(updateTracking.getTrackingNumber());
                }
            } catch (TrackingMoreException e) {
                System.err.println("error：" + e.getMessage());
            } catch (IOException e) {
                System.err.println("error：" + e.getMessage());
            }
        }
    }
