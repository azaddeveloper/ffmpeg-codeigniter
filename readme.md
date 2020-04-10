 // change size of the video
        $ffmpeg_array = array(  
            'ffmpeg.binaries' => FCPATH . $this->config->item('ffmpeg') , /////////,
            'ffprobe.binaries' => FCPATH . $this->config->item('ffprobe') ,
            'timeout' => 9800, // The timeout for the underlying process
            'ffmpeg.threads' => 1
        );
       
        $input='C:\Users\sundar azad\Desktop\images\video\golden.mp4';
        $output='C:\Users\sundar azad\Desktop\images\video\testvideo1.mp4';

        $ffmpeg = \FFMpeg\FFMpeg::create($ffmpeg_array);
        $video = $ffmpeg->open($input);;
               
         $video->filters()
               ->resize(new FFMpeg\Coordinate\Dimension(720 , 480)) // put size of the video
                ->synchronize();




        $format = new FFMpeg\Format\Video\X264('aac');
        $format->setKiloBitrate(800);
        $format->setAudioCodec($format->getAvailableAudioCodecs()[0]);
        $format->setAudioChannels(2);
        $format->setAudioKiloBitrate(128);

        $video->save($format,$output);
                
                
  // tabk image form the video              
  
  
   $video->filters()->resize(new FFMpeg\Coordinate\Dimension(320, 240))->synchronize();
        $video->save(new FFMpeg\Format\Video\X264('aac') , 'minified-intro.mp4');
        $video = $ffmpeg->open('minified-intro.mp4');
        $video->frame(FFMpeg\Coordinate\TimeCode::fromSeconds(1))->save($output);
  
  
  
