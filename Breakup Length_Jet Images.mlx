% Define the directory where your images are stored
folder = uigetdir("D:\MATLAB\Jet_images");

% Get a list of all image files in the directory
imageFiles = dir(fullfile(folder, '*.tif'));

% Initialize an array to store breakup lengths
breakupLengths = zeros(1, numel(imageFiles));

% Loop through each image
for i = 1:numel(imageFiles)
    % Read the image
    filename = fullfile(folder, imageFiles(i).name);
    image = imread(filename);
    %Background image Subtraction
    bg = imread("bg.tif");
    images = bg - image

    % Binarize the image
    binaryImage = imbinarize(images,0.17);

    % Find the bounding boxes of all bright patches in the binary imagebreakupLengths
    props = regionprops(binaryImage, 'BoundingBox');

    % Initialize variables to store the maximum area and corresponding bounding box
    maxArea = 0;
    maxY0 = 0;
    maxX0 = 0;
    maxHeight = 0;
    maxWidth = 0;

    % Loop through all bright patches
    for j = 1:length(props)
        % Get the bounding box of the current bright patch
        boundingBox = props(j).BoundingBox;
        y0 = boundingBox(2);
        x0 = boundingBox(1);
        height = boundingBox(4);
        width = boundingBox(3);

        % Calculate the area of the current bright patch
        area = height * width;

        % Check if the current bright patch has a larger area than the previous largest area
        if area > maxArea
            % Update the maximum area and corresponding bounding box
            maxArea = area;
            maxY0 = y0;
            maxX0 = x0;
            maxHeight = height;
            maxWidth = width;
        end
    end

    % Calculate the breakup length as the height of the largest bright patch
    breakupLength = maxHeight;

    % Store the breakup length for the current image
    breakupLengths(i) = breakupLength;

    % Display the results for the current image
    fprintf('Image %d: Breakup length = %d pixels\n', i, breakupLength);
end

% Display the average breakup length for all images
fprintf('Average breakup length: %f pixels\n', mean(breakupLengths));

