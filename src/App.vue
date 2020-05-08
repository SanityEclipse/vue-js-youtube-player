<template>
    <div class="container">
        <SearchBar 
            @searchTermChange= "onSearchTermChange"
        ></SearchBar>
        <div class="row">
            <VideoDetail 
                :video= "selectedVideo"
            ></VideoDetail>

            <VideoList 
                @videoSelect= "onVideoSelect"
                :videos= "videos"
            ></VideoList>
        </div>
    </div>
</template>

<script>
    import SearchBar from './components/SearchBar';
    import VideoList from './components/VideoList';
    import VideoDetail from './components/VideoDetail';

    import axios from 'axios'; 

    // Toggle this setting to switch between test data and live API calls
    const DEV_MODE = true; 
    const API_KEY = '';

    export default {
        name: 'App',
        components: {
            SearchBar,
            VideoList,
            VideoDetail
        },
        data() {
            return {
                videos: [],
                selectedVideo: null
            };
        },
        methods: {
            onSearchTermChange(searchTerm) {
                // YouTube limits the number of API calls made in a day. Use this for testing. 
                // This custom data replicates the info queried from YouTube, and for developement
                // purposes, is interchangable with the live data. 
                if(DEV_MODE) {
                    console.info("Search term is: " + searchTerm)
                    this.useTestData(); 
                } else {
                    axios.get('https://www.googleapis.com/youtube/v3/search', {
                        params: {
                            key: API_KEY,
                            type: 'video',
                            part: 'snippet',
                            q: searchTerm
                        }
                    }).then(response => {
                        this.videos = response.data.items
                    })
                }
            },
            onVideoSelect(video){
                this.selectedVideo = video;
            },
            useTestData() {
                return this.videos =                  
                    [
                        {
                            etag: 0,
                            snippet: {
                                title: "movie 1",
                                thumbnails: {
                                    default: {
                                        url: "https://via.placeholder.com/120x90.png?text=Video+Thumbnail"
                                    }
                                },
                                description: "This is movie #1."
                            },
                            id: {
                                videoId: "video_1"
                            }
                        },
                        {
                            etag: 1,
                            snippet: {
                                title: "movie 2",
                                thumbnails: {
                                        default: {
                                            url: "https://via.placeholder.com/120x90.png?text=Video+Thumbnail"
                                    }
                                },
                                description: "This is movie #2."
                            },
                            id: {
                                videoId: "video_2"
                            }
                        },
                        {
                            etag: 2,
                            snippet: {
                                title: "movie 3",
                                thumbnails: {
                                        default: {
                                            url: "https://via.placeholder.com/120x90.png?text=Video+Thumbnail"
                                    }
                                },
                                description: "This is movie #3.",
                            },
                            id: {
                                videoId: "video_3"
                            }
                        },
                        {
                            etag: 3,
                            snippet: {
                                title: "movie 4",
                                thumbnails: {
                                        default: {
                                            url: "https://via.placeholder.com/120x90.png?text=Video+Thumbnail"
                                    }
                                },
                                description: "This is movie #4."
                            },
                            id: {
                                videoId: "video_4"
                            }
                        },
                        {
                            etag: 4,
                            snippet: {
                                title: "movie 5",
                                thumbnails: {
                                    default: {
                                        url: "https://via.placeholder.com/120x90.png?text=Video+Thumbnail"
                                    }
                                },
                                description: "This is movie #5."
                            },
                            id: {
                                videoId: "video_5"
                            }
                        }
                    ]
            }   
        }    
    };
</script>