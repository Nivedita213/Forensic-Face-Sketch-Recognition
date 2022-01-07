# Forensic-Face-Sketch-Recognition
Minor Project
function output_value = load_database();
persistent loaded;
persistent numeric_Image;
if(isempty(loaded))
    all_Images = zeros(10304,40);
    for i=1:40
        cd(strcat('s',num2str(i)));
        for j=1:10
            image_Container = imread(strcat(num2str(j),'.pgm'));
            all_Images(:,(i-1)*10+j)=reshape(image_Container,size(image_Container,1)*size(image_Container,2),1);
        end
        display('Doading Database');
        cd ..
    end
    numeric_Image = uint8(all_Images);
end
loaded = 1;
output_value = numeric_Image







loaded_Image=load_database();
random_Index=round(400*rand(1,1));          
random_Image=loaded_Image(:,random_Index);                         
rest_of_the_images=loaded_Image(:,[1:random_Index-1 random_Index+1:end]);         
image_Signature=20;                            
white_Image=uint8(ones(1,size(rest_of_the_images,2)));
mean_value=uint8(mean(rest_of_the_images,2));                
mean_Removed=rest_of_the_images-uint8(single(mean_value)*single(white_Image)); 
L=single(mean_Removed)'*single(mean_Removed);
[V,D]=eig(L);
V=single(mean_Removed)*V;
V=V(:,end:-1:end-(image_Signature-1));          
all_image_Signatire=zeros(size(rest_of_the_images,2),image_Signature);
for i=1:size(rest_of_the_images,2);
    all_image_Signatire(i,:)=single(mean_Removed(:,i))'*V;  
end
subplot(121);
imshow(reshape(random_Image,112,92));
title('Looking for this Face','FontWeight','bold','Fontsize',16,'color','red');
subplot(122);
p=random_Image-mean_value;
s=single(p)'*V;
z=[];
for i=1:size(rest_of_the_images,2)
    z=[z,norm(all_image_Signatire(i,:)-s,2)];
    if(rem(i,20)==0),imshow(reshape(rest_of_the_images(:,i),112,92)),end;
    drawnow;
end
[a,i]=min(z);
subplot(122);
imshow(reshape(rest_of_the_images(:,i),112,92));
title('Recognition Completed','FontWeight','bold','Fontsize',16,'color','red')
